# Robotic Control & Visualization Software

A Linux-based desktop application for controlling, monitoring, and visualizing a mechanical hand / robotic system in real time.

The application targets **Ubuntu Linux** and **Raspberry Pi OS** and provides manual robot control, controller communication, 3D visualization, G-code execution, diagnostics, and integration with existing RTOS/SLAM interfaces.

> **Status:** In development  
> **Duration:** 10 weeks  
> **Target completion:** 25 October 2026  
> **Project cost:** ₹35,000

## 1. Project Overview

The software acts as a high-level control and visualization station between the operator and the existing robotic controller.

```text
Operator
   ↓
Linux Desktop Application
   ↓
Control Engine
   ↓
Hardware Abstraction Layer
   ↓
Existing Controller / RTOS
   ↓
Motors / Sensors / Encoders
```

The Linux application is responsible for high-level control, visualization, configuration, and supervision. Deterministic low-level motor control remains with the existing controller/RTOS.

## 2. Core Features

### Manual Robot Control

- X / X-
- Y / Y-
- Z / Z-
- A / A-
- B / B-
- C / C-
- Directional movement controls
- Configurable movement step
- Configurable movement speed
- Current position
- Target position
- Start / Stop controls
- Axis-limit validation

### Real-Time Machine Monitoring

Where supported by the controller:

- Current X/Y/Z position
- Current A/B/C orientation
- Movement state
- Controller state
- Connection status
- Available sensor telemetry
- Controller errors

### Interactive 3D Visualization

- Mechanical hand / robot model
- 3D viewport
- Camera rotation
- Zoom and pan
- Coordinate axes
- Grid
- Current position
- Target position
- Movement/path visualization

The required CAD/3D model and robot geometry are expected to be provided by the project owner.

### G-Code Workflow

```text
G-Code File
    ↓
Parser
    ↓
Validator
    ↓
Path Generation
    ↓
Preview
    ↓
Command Queue
    ↓
Controller
```

Supported workflow:

- G-code upload
- Parsing
- Validation
- Path preview
- Execution
- Progress tracking
- Pause
- Resume
- Stop
- Current command/line status

The exact G-code command set will be finalized according to the controller requirements.

### Motion Management

- Command queue
- Sequential execution
- Basic motion interpolation
- Movement status
- Controller acknowledgement where supported
- Configurable movement parameters

### Machine Configuration

- Axis limits
- Movement step
- Speed
- Communication settings
- Machine parameters
- Controller parameters

### Error Handling & Diagnostics

Common software and communication failures will be handled, including:

- Controller disconnection
- Command timeout
- Invalid coordinates
- Axis-limit violation
- Invalid G-code
- Controller errors

Diagnostics include command logs, controller responses, connection events, errors, G-code execution events, and system events.

### RTOS Integration

Integration with an existing RTOS/controller interface can expose controller state, motor status, error information, and available telemetry.

### SLAM Integration

Where an existing SLAM system or API is available, the application can integrate robot position, localization, mapping data, and SLAM status.

## 3. Technology Stack

| Component | Technology |
|---|---|
| Programming Language | Python |
| Desktop Framework | PySide6 / Qt 6 |
| 3D Visualization | PyVista + VTK |
| Configuration | YAML |
| Logging | Python `logging` |
| Testing | pytest |
| Communication | Serial / USB / TCP/IP / CAN / ROS2 |
| Version Control | Git / GitHub |
| Deployment | Linux package / installer |
| Documentation | Markdown |

The final communication adapter will depend on the controller supplied by the project owner.

## 4. Proposed Architecture

```text
┌───────────────────────────────────────────────────────┐
│                    PRESENTATION                       │
│                                                       │
│ Manual Control │ G-Code │ 3D │ RTOS │ SLAM          │
│ Configuration  │ Diagnostics │ Machine Status        │
└─────────────────────────┬─────────────────────────────┘
                          ↓
┌───────────────────────────────────────────────────────┐
│                  APPLICATION LAYER                    │
│                                                       │
│ Command Manager │ Motion Manager │ G-Code Manager     │
│ Robot State Manager │ Configuration Manager           │
└─────────────────────────┬─────────────────────────────┘
                          ↓
┌───────────────────────────────────────────────────────┐
│                    CONTROL ENGINE                     │
│                                                       │
│ Command Queue │ Interpolation │ Axis Limits          │
│ State Machine │ Feedback │ Error Handling             │
└─────────────────────────┬─────────────────────────────┘
                          ↓
┌───────────────────────────────────────────────────────┐
│              HARDWARE ABSTRACTION LAYER              │
│                                                       │
│ Serial │ USB │ TCP/IP │ CAN │ ROS2 Adapter           │
└─────────────────────────┬─────────────────────────────┘
                          ↓
                 Existing Controller
                          │
                ┌─────────┼─────────┐
                ↓         ↓         ↓
              Motors    Sensors   Encoders
```

### Control Flow

```text
User Input
   ↓
UI
   ↓
Motion Service
   ↓
Command Validation
   ↓
Axis Limit Check
   ↓
Command Queue
   ↓
Motion Manager
   ↓
Hardware Adapter
   ↓
Controller
   ↓
Robot
```

Feedback follows the reverse path through the controller, hardware adapter, feedback processor, robot state, and UI/3D visualization.

## 5. Proposed Project Structure

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
├── docs/
├── requirements.txt
├── README.md
└── pyproject.toml
```

## 6. Robot State Management

The application should maintain a centralized robot state rather than allowing individual UI components to maintain independent values.

Example:

```python
RobotState(
    connected=True,
    machine_state="MOVING",
    x=120.5,
    y=80.0,
    z=150.0,
    a=20.0,
    b=10.0,
    c=45.0,
)
```

The 3D visualization, status panels, diagnostics, and control interface can consume the same state.

## 7. Robot State Machine

```text
                ┌──────────────┐
                │ DISCONNECTED │
                └──────┬───────┘
                       │ connect
                       ↓
                ┌──────────────┐
                │     IDLE     │
                └──────┬───────┘
                       │ command
                       ↓
                ┌──────────────┐
                │    MOVING    │
                └──────┬───────┘
                       │ complete
                       ↓
                ┌──────────────┐
                │     IDLE     │
                └──────────────┘

              Error ───────→ FAULT
                                │
                                ↓
                             RECOVERY
```

## 8. Configuration

Machine-specific parameters should be configurable rather than hardcoded.

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

Actual values will be defined after receiving the machine specifications.

## 9. Development Timeline

**Development period:** 16 August 2026 – 25 October 2026

| Period | Phase | Main Deliverables | Payment |
|---|---|---|---:|
| 16 Aug | Project Kickoff & Technical Discovery | Hardware, controller, protocol, RTOS and requirements | ₹10,500 |
| 17–23 Aug | Application Foundation | PySide6 app, architecture, navigation, configuration, logging | — |
| 24–30 Aug | Manual Control | X/Y/Z, A/B/C, directional controls, speed, step size | — |
| 31 Aug–6 Sep | Hardware Communication | Controller connection, commands, feedback, status | ₹8,750 |
| 7–13 Sep | Control Engine | Queue, state management, interpolation, validation | — |
| 14–20 Sep | Feedback & Diagnostics | Position, status, timeout handling, recovery, logs | — |
| 21–27 Sep | 3D Visualization | Model, coordinates, transformation, path visualization | ₹8,750 |
| 28 Sep–4 Oct | G-Code | Upload, parsing, validation, preview, execution queue | — |
| 5–11 Oct | Execution & Integrations | Progress, pause/resume/stop, RTOS/SLAM interfaces | ₹3,500 |
| 12–18 Oct | Testing & Optimization | Hardware, G-code, Ubuntu and Raspberry Pi testing | — |
| 19–25 Oct | Deployment & Handover | Builds, deployment, documentation, source handover | ₹3,500 |

## 10. Payment Schedule

| Payment Date | Milestone | Amount | Cumulative |
|---|---|---:|---:|
| **16 August 2026** | Project initiation | **₹10,500** | ₹10,500 |
| **6 September 2026** | Hardware communication milestone | **₹8,750** | ₹19,250 |
| **27 September 2026** | 3D + integrated control milestone | **₹8,750** | ₹28,000 |
| **11 October 2026** | Core feature completion | **₹3,500** | ₹31,500 |
| **25 October 2026** | Final deployment & handover | **₹3,500** | **₹35,000** |

## 11. Client Inputs Required

The following information/materials are required during the relevant development phases:

- Mechanical hand/robot specifications
- Controller/MCU details
- Communication protocol documentation
- Existing firmware/RTOS interface
- Hardware access for testing
- CAD/3D model
- Robot geometry
- Coordinate definitions
- Axis limits
- Kinematic information, if available
- G-code specification and sample files
- Existing SLAM interface, if applicable
- Representative test cases
- Expected machine behavior

Timely availability of hardware, documentation, and interfaces is necessary to maintain the planned development schedule.

## 12. Final Deliverables

At project completion:

- Ubuntu-compatible desktop application
- Raspberry Pi OS-compatible deployment
- Manual X/Y/Z/A/B/C controls
- Real-time position and machine-status display where supported
- Hardware communication integration
- Interactive 3D visualization
- Movement and G-code path visualization
- G-code upload, validation, parsing and execution
- Command queue and basic motion interpolation
- Machine configuration and axis-limit management
- Error handling and diagnostics
- Logging system
- RTOS/controller status integration where an existing interface is available
- SLAM data/status integration where an existing interface is available
- Installation and technical documentation
- Source-code handover

## 13. Development Principles

- Modular architecture
- Separation of UI and hardware logic
- Hardware abstraction
- Centralized robot state
- Explicit command validation
- Fail-safe software behavior
- Clear logging and diagnostics
- Configuration-driven machine parameters
- Testable control logic
- Linux-first deployment
- Extensibility for future hardware and robotics integrations

## 14. Development Setup

A typical development environment will use:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Run the application during development with:

```bash
python -m app.main
```

Final commands may change based on the selected packaging and deployment strategy.

## 15. Project Milestones

```text
16 Aug  → Technical Discovery
    ↓
30 Aug  → Application + Manual Control
    ↓
06 Sep  → Hardware Communication
    ↓
20 Sep  → Control + Feedback + Diagnostics
    ↓
27 Sep  → 3D Visualization
    ↓
11 Oct  → G-Code + Integrations
    ↓
18 Oct  → Testing
    ↓
25 Oct  → Deployment & Handover
```

## 16. Project Information

**Project:** Robotic Control & Visualization Software  
**Prepared for:** Aditya Sir  
**Developer:** Vinayak  
**Cost:** ₹35,000  
**Duration:** 10 weeks  
**Start:** 16 August 2026  
**Target Completion:** 25 October 2026

---

This README describes the proposed software architecture and development plan. Hardware-specific implementation details will be finalized after the controller, communication protocol, robot specifications, and available interfaces are confirmed.
