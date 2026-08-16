# Software Architecture

## 1. Architecture Goal

The system should separate presentation, application logic, control logic and hardware communication.

```text
UI
 ↓
Application Services
 ↓
Control Engine
 ↓
Hardware Abstraction
 ↓
Controller / RTOS
 ↓
Robot
```

## 2. Layers

### Presentation

PySide6/Qt UI.

Responsibilities:

- Input
- Display
- Visualization
- Status
- User feedback

### Application Layer

Responsibilities:

- Command orchestration
- Motion services
- G-code services
- Configuration
- Robot state coordination

### Control Engine

Responsibilities:

- Command validation
- Queueing
- State machine
- Basic interpolation
- Feedback processing
- Error handling

### Hardware Abstraction

Responsibilities:

- Connect/disconnect
- Command transport
- Feedback reception
- Protocol adaptation
- Timeout handling

### Controller

The existing controller/RTOS remains responsible for deterministic low-level control.

---

## 3. Core Data Flow

### Manual Control

```text
Button
 ↓
Motion Service
 ↓
Validation
 ↓
Command Queue
 ↓
Motion Manager
 ↓
Hardware Adapter
 ↓
Controller
```

### Feedback

```text
Controller
 ↓
Hardware Adapter
 ↓
Feedback Processor
 ↓
Robot State
 ├── UI
 ├── 3D
 └── Diagnostics
```

### G-Code

```text
File
 ↓
Parser
 ↓
Intermediate Representation
 ↓
Validator
 ↓
Motion Commands
 ↓
Queue
 ↓
Controller
```

## 4. State Machine

```text
DISCONNECTED → IDLE → MOVING → IDLE
                    ├→ PAUSED
                    ├→ STOPPING
                    └→ FAULT → RECOVERY
```

## 5. Hardware Abstraction

Define a common interface such as:

```python
class HardwareInterface:
    def connect(self): ...
    def disconnect(self): ...
    def send_command(self, command): ...
    def read_feedback(self): ...
    def is_connected(self): ...
```

Adapters can then implement:

- Serial
- TCP/IP
- CAN
- ROS2

Only the required adapter should be implemented initially.

## 6. 3D Visualization

3D rendering consumes robot state:

```text
Robot State
 ↓
Coordinate Transformation
 ↓
Robot Model
 ↓
PyVista / VTK
 ↓
Viewport
```

The renderer must not directly control hardware.

## 7. Configuration

Machine-specific configuration belongs in YAML or equivalent external configuration.

## 8. Concurrency

Blocking hardware/network operations must not block the Qt event loop.

Use Qt worker threads, signals/slots or an appropriate asynchronous architecture.

## 9. Architecture Constraints

- No hardware access from UI widgets.
- No controller-specific packet generation in UI code.
- No machine limits hardcoded into presentation code.
- No hard real-time motor-control loop in the Linux UI process.
- Centralize robot state.
