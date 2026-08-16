# AGENTS.md

## Project

This repository contains a Linux-based desktop application for controlling, monitoring, and visualizing a mechanical hand / robotic system.

Target platforms:

- Ubuntu Linux
- Raspberry Pi OS

The application provides:

- Manual X/Y/Z/A/B/C control
- Directional movement controls
- Real-time robot/controller status
- Hardware communication
- Interactive 3D visualization
- Movement/path visualization
- G-code upload, parsing, validation, and execution
- Command queue and basic motion interpolation
- Machine configuration
- Logging and diagnostics
- Existing RTOS/controller integration
- Existing SLAM integration where available

The application is a high-level control and visualization station. Deterministic low-level motor control should remain with the existing controller/MCU/RTOS.

---

## 1. Engineering Principles

All development in this repository should follow these principles:

1. Prefer simple, explicit, maintainable designs over premature abstraction.
2. Keep UI code separate from robot-control and hardware communication logic.
3. Never allow UI widgets to communicate directly with hardware.
4. Centralize robot state.
5. Validate all movement commands before transmission.
6. Treat hardware communication as unreliable.
7. Fail safely when communication or controller state is unknown.
8. Never silently ignore controller errors.
9. Keep machine-specific configuration outside application logic.
10. Write code that can be tested without physical hardware where practical.
11. Avoid blocking the Qt UI thread.
12. Keep hardware-specific implementations behind interfaces/adapters.
13. Do not introduce a new dependency unless it provides clear value.
14. Prefer standard Python facilities when they are sufficient.
15. Keep the application deployable on both Ubuntu and Raspberry Pi OS.

---

## 2. Architecture

The expected architecture is:

```text
┌──────────────────────────────────────────────┐
│                 PRESENTATION                 │
│                                              │
│ Manual │ G-Code │ 3D │ RTOS │ SLAM          │
│ Config │ Diagnostics │ Machine Status       │
└───────────────────────┬──────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────┐
│              APPLICATION LAYER               │
│                                              │
│ Command Manager                              │
│ Motion Manager                               │
│ G-Code Manager                               │
│ Robot State Manager                          │
│ Configuration Manager                        │
└───────────────────────┬──────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────┐
│                CONTROL ENGINE                │
│                                              │
│ Command Queue │ Interpolation │ Limits       │
│ State Machine │ Feedback │ Error Handling    │
└───────────────────────┬──────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────┐
│          HARDWARE ABSTRACTION LAYER          │
│                                              │
│ Serial │ USB │ TCP/IP │ CAN │ ROS2           │
└───────────────────────┬──────────────────────┘
                        │
                        ▼
                Existing Controller
                        │
                ┌───────┼───────┐
                ▼       ▼       ▼
              Motors  Sensors Encoders
```

### Critical boundary

The Linux application must not be treated as the deterministic motor-control loop.

Use:

```text
Linux application
    = high-level commands + visualization + supervision

Controller / MCU / RTOS
    = deterministic low-level control
```

Do not move hard real-time responsibilities into Python unless explicitly required and technically validated.

---

## 3. Repository Structure

Use a modular structure similar to:

```text
robot-control/
│
├── app/
│   ├── main.py
│   └── application.py
│
├── ui/
│   ├── main_window.py
│   ├── manual_control.py
│   ├── gcode_view.py
│   ├── robot_view.py
│   ├── machine_config.py
│   ├── rtos_view.py
│   ├── slam_view.py
│   └── diagnostics.py
│
├── core/
│   ├── robot_state.py
│   ├── command_manager.py
│   ├── state_machine.py
│   └── events.py
│
├── motion/
│   ├── motion_manager.py
│   ├── interpolator.py
│   ├── trajectory.py
│   └── limits.py
│
├── gcode/
│   ├── parser.py
│   ├── validator.py
│   ├── executor.py
│   └── preview.py
│
├── hardware/
│   ├── interface.py
│   ├── serial_adapter.py
│   ├── tcp_adapter.py
│   ├── can_adapter.py
│   └── protocol.py
│
├── robot/
│   ├── coordinate_system.py
│   ├── kinematics.py
│   └── robot_model.py
│
├── integrations/
│   ├── rtos.py
│   ├── ros2.py
│   └── slam.py
│
├── safety/
│   ├── validator.py
│   ├── watchdog.py
│   └── emergency_stop.py
│
├── config/
│   └── machine.yaml
│
├── tests/
│
├── docs/
│
├── requirements.txt
├── pyproject.toml
├── README.md
└── AGENTS.md
```

Do not create directories or abstractions solely for appearance. Adjust the structure as implementation becomes concrete.

---

## 4. UI Rules

Use **PySide6 / Qt 6**.

### UI responsibilities

The UI should:

- Display state.
- Accept user input.
- Trigger application services.
- Display validation errors.
- Display connection/controller status.
- Display robot visualization.
- Display G-code execution state.

The UI should not:

- Open serial ports directly.
- Construct raw hardware protocol packets.
- Perform robot motion calculations directly.
- Maintain independent copies of robot state.
- Contain business/control logic.

Bad:

```python
def on_x_plus_clicked(self):
    self.serial.write(b"X+10")
```

Preferred:

```python
def on_x_plus_clicked(self):
    self.motion_service.move_relative(axis="X", distance=10)
```

The service/control layer should handle validation and hardware communication.

---

## 5. Robot State

Maintain a single authoritative robot state.

A conceptual model:

```python
@dataclass
class RobotState:
    connected: bool
    machine_state: str
    x: float
    y: float
    z: float
    a: float
    b: float
    c: float
```

The actual model may evolve.

The following components should consume the same state:

```text
Robot State
   ├── Manual Control
   ├── Status Panel
   ├── 3D Visualization
   ├── G-Code Execution
   ├── Diagnostics
   └── RTOS/SLAM Integration
```

Do not allow different components to independently assume the current robot position.

---

## 6. Coordinate System

The exact coordinate definition must come from the project owner.

Do not assume:

- Axis direction
- Coordinate origin
- Units
- Rotation convention
- Joint mapping
- Robot/world/tool coordinate frames

Before implementing movement logic, document:

```text
X:
Y:
Z:

A:
B:
C:

Units:
Origin:
Positive direction:
Minimum:
Maximum:
```

Any change to the coordinate system can affect UI controls, G-code, visualization, limits, and motion logic.

---

## 7. Hardware Communication

Hardware communication must be isolated behind an interface.

Conceptually:

```python
class HardwareInterface:
    def connect(self): ...
    def disconnect(self): ...
    def send_command(self, command): ...
    def read_feedback(self): ...
    def is_connected(self): ...
```

Concrete implementations may include:

```text
SerialAdapter
TCPAdapter
CANAdapter
ROS2Adapter
```

Only the selected protocol should be implemented initially.

### Rules

- Never duplicate protocol logic across UI components.
- Handle connection failures explicitly.
- Apply timeouts.
- Validate outgoing commands.
- Validate incoming data.
- Log communication errors.
- Never assume a command was executed without acknowledgement when acknowledgement is part of the protocol.
- Avoid blocking the UI thread.

---

## 8. Motion Control

All motion commands should follow:

```text
Input
  ↓
Command Validation
  ↓
Axis Limit Check
  ↓
Robot State Check
  ↓
Command Queue
  ↓
Motion Manager
  ↓
Hardware Adapter
  ↓
Controller
```

Validate:

- Axis name
- Numeric values
- Axis limits
- Allowed machine state
- Command format
- Speed limits
- Controller availability

Never send an unchecked user input directly to the controller.

---

## 9. State Machine

Use explicit machine states rather than scattered boolean flags.

Expected states:

```text
DISCONNECTED
IDLE
MOVING
PAUSED
STOPPING
FAULT
RECOVERY
```

Actual states should follow the controller's capabilities.

State transitions should be deterministic and documented.

Example:

```text
DISCONNECTED
     │
     │ connect
     ▼
   IDLE
     │
     │ command
     ▼
  MOVING
     │
     ├── complete ──> IDLE
     │
     ├── pause ─────> PAUSED
     │
     ├── stop ──────> STOPPING
     │
     └── error ─────> FAULT
```

---

## 10. G-Code

G-code should be processed independently of the UI.

Pipeline:

```text
File
 ↓
Parser
 ↓
Intermediate Command Representation
 ↓
Validator
 ↓
Path / Motion Generation
 ↓
Command Queue
 ↓
Controller
```

Do not couple the parser directly to the hardware protocol.

The parser should convert G-code into internal commands.

Example:

```text
G1 X100 Y50 Z20 F100
```

should become an internal movement representation before controller-specific encoding.

This allows the same command system to support:

- Manual movement
- G-code movement
- Future automated movement

---

## 11. 3D Visualization

The 3D renderer should consume robot state.

Preferred flow:

```text
Controller
   ↓
Robot State
   ↓
3D Transformation
   ↓
Robot Model
   ↓
Viewport
```

Do not make the 3D viewport communicate directly with the hardware.

The 3D model should reflect known robot state. It should not be treated as the authoritative source of physical position.

Required capabilities:

- Model loading
- Camera controls
- Coordinate axes
- Grid
- Current position
- Target position
- Basic movement path

Advanced collision detection and advanced trajectory visualization should only be implemented when explicitly required.

---

## 12. Configuration

Machine-specific settings belong in configuration files.

Example:

```yaml
machine:
  name: MechanicalHand

communication:
  type: serial
  port: /dev/ttyUSB0
  baudrate: 115200

axes:
  x:
    min: -100
    max: 100
    unit: mm

  y:
    min: -100
    max: 100
    unit: mm

  z:
    min: 0
    max: 200
    unit: mm
```

Never hardcode production machine limits in UI components.

Validate configuration at application startup.

---

## 13. Error Handling

Errors must be explicit and actionable.

Examples:

```text
Controller disconnected
Command timeout
Axis limit exceeded
Invalid coordinate
Invalid G-code
Controller fault
Unsupported command
Invalid configuration
```

Avoid generic errors such as:

```text
Something went wrong.
```

Prefer:

```text
Controller connection lost.
Active commands have been stopped.
Check the controller connection and reconnect.
```

Log the underlying technical error while showing an appropriate user-facing message.

---

## 14. Logging

Use Python's `logging` infrastructure.

Recommended levels:

```text
DEBUG
INFO
WARNING
ERROR
CRITICAL
```

Log:

- Application startup/shutdown
- Controller connection/disconnection
- Commands sent
- Important controller responses
- Errors
- G-code execution
- State transitions
- Configuration errors

Avoid logging:

- Passwords
- Tokens
- Secrets
- Unnecessary high-frequency telemetry

Logs should be useful for diagnosing field failures.

---

## 15. Threading & Concurrency

The Qt UI thread must remain responsive.

Potential blocking operations include:

- Serial reads
- Network reads
- CAN communication
- Long G-code execution
- File parsing
- Large 3D model loading

Move blocking operations to appropriate worker threads or asynchronous execution.

Do not call blocking hardware operations directly from UI callbacks.

Use Qt signals/slots or an appropriate thread-safe event mechanism to return results to the UI.

---

## 16. Testing

Use `pytest`.

Prioritize testing of:

### Unit tests

- Coordinate validation
- Axis limits
- Command generation
- G-code parser
- G-code validator
- State transitions
- Configuration validation
- Motion interpolation

### Integration tests

- Hardware adapter
- Controller protocol
- Feedback processing
- Command queue
- G-code execution flow

### Hardware tests

When physical hardware is available:

- Connection
- Position commands
- Feedback
- Stop
- Pause/resume
- Controller restart
- Communication failure
- Axis boundaries
- Long-running execution

Hardware tests must follow the project's agreed safety procedures.

---

## 17. Safety

This application controls physical machinery.

Never bypass hardware safety systems for convenience.

Software should:

- Validate movement limits.
- Detect communication failures.
- Stop/clear commands when required.
- Handle controller faults.
- Avoid sending commands when controller state is unknown.

Hardware-level safety mechanisms such as emergency stop, motor protection, limit switches and controller-level safety should remain part of the machine's hardware/control architecture.

Do not claim that software validation alone provides safety certification.

---

## 18. RTOS Integration

RTOS functionality is an integration boundary.

Expected architecture:

```text
Linux Application
       ↓
RTOS Adapter
       ↓
Existing RTOS / Controller
       ↓
Hardware
```

Do not implement or modify RTOS firmware unless explicitly requested and separately scoped.

Before implementing the integration, obtain:

- Communication/API documentation
- Controller states
- Telemetry specification
- Error codes
- Command format
- Firmware version information

---

## 19. SLAM Integration

SLAM is treated as an integration point.

Expected architecture:

```text
Existing SLAM System
        ↓
ROS2 / API / Network
        ↓
SLAM Adapter
        ↓
Robot State
        ↓
Visualization
```

Do not implement a new SLAM algorithm unless explicitly required.

Before integration, identify:

- SLAM framework
- Data source
- API/topic interface
- Coordinate frames
- Map format
- Localization format
- Required UI information

---

## 20. Deployment

The application must be deployable on:

- Ubuntu Linux
- Raspberry Pi OS

Before release, test:

```text
Fresh Ubuntu installation
Fresh Raspberry Pi OS installation
Application startup
Controller connection
Configuration loading
3D visualization
G-code workflow
Logging
Application shutdown
```

Avoid relying on developer-machine-specific paths.

Use environment/configuration values for machine-specific resources.

---

## 21. Git Workflow

Use small, focused commits.

Preferred format:

```text
feat: add manual axis control
feat: add serial controller adapter
feat: add robot state manager
fix: handle controller timeout
fix: validate axis limits
refactor: isolate hardware protocol
test: add gcode parser tests
docs: update deployment guide
```

Avoid large commits containing unrelated changes.

Do not commit:

```text
.env
*.secret
credentials
API keys
large generated binaries
machine-specific private data
```

---

## 22. Dependency Management

Pin or constrain important production dependencies.

Keep dependencies minimal.

Before adding a package, consider:

1. Is it necessary?
2. Is it maintained?
3. Is it compatible with Raspberry Pi OS?
4. Does it support the required Python version?
5. Does it increase deployment complexity?
6. Can the functionality be implemented safely with the standard library?

Avoid dependencies that are difficult to build on ARM unless there is a clear benefit.

---

## 23. Code Quality

Follow standard Python practices:

- Type hints for public interfaces.
- Descriptive names.
- Small focused functions.
- Explicit error handling.
- No unnecessary global state.
- No duplicated protocol logic.
- No magic numbers for machine parameters.
- Docstrings for non-obvious public APIs.
- Keep UI classes from becoming large monoliths.

Prefer composition over inheritance where practical.

---

## 24. Change Management

Before implementing a new feature:

1. Identify which architectural layer owns the feature.
2. Check whether an existing service/module can support it.
3. Define the interface before implementing the concrete integration.
4. Add tests for important control logic.
5. Update documentation when behavior or configuration changes.
6. Verify Ubuntu compatibility.
7. Verify Raspberry Pi compatibility when the change affects deployment or hardware communication.

Do not solve architecture problems by adding logic directly to the UI.

---

## 25. Development Milestones

### Milestone 1 — 16 August

Technical discovery and architecture.

### Milestone 2 — 30 August

Application foundation and manual control.

### Milestone 3 — 6 September

Working hardware communication.

### Milestone 4 — 20 September

Control, feedback and diagnostics.

### Milestone 5 — 27 September

3D visualization integrated.

### Milestone 6 — 11 October

G-code workflow and RTOS/SLAM integrations.

### Milestone 7 — 18 October

System testing.

### Milestone 8 — 25 October

Deployment and handover.

---

## 26. Definition of Done

A feature is considered complete when:

- The intended behavior is implemented.
- Input validation is present where required.
- Errors are handled appropriately.
- Important logic has tests.
- UI remains responsive.
- No direct hardware access exists in presentation components.
- Documentation/configuration is updated where necessary.
- The feature works with the agreed controller interface.
- Ubuntu compatibility is verified.
- Raspberry Pi compatibility is verified when applicable.

---

## 27. Current Project Constraints

The final implementation depends on information and resources supplied by the project owner, including:

- Mechanical hand specifications
- Controller/MCU
- Communication protocol
- Existing firmware/RTOS interface
- Hardware access
- CAD/3D model
- Robot geometry
- Coordinate definitions
- Axis limits
- Kinematic information
- G-code specification and examples
- Existing SLAM interface

These should be confirmed before implementing the dependent functionality.

---

## 28. Final Project Information

**Project:** Robotic Control & Visualization Software  
**Developer:** Vinayak  
**Project Owner:** Aditya Sir  
**Budget:** ₹35,000  
**Duration:** 10 weeks  
**Start Date:** 16 August 2026  
**Target Completion:** 25 October 2026

This document is the engineering guidance for AI coding agents and developers working on the project. It should be updated when the hardware protocol, architecture, technology choices, or development conventions are finalized.
