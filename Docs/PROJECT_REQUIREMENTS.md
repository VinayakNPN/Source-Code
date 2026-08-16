# Project Requirements Specification

## 1. Purpose

This document defines the functional and technical requirements for the Linux-based Robotic Control & Visualization Software.

**Project Owner:** Aditya Sir  
**Developer:** Vinayak  
**Budget:** ₹35,000  
**Development Period:** 16 August 2026 – 25 October 2026

Requirements are intentionally written so that hardware-dependent details can be finalized after controller documentation and hardware access are provided.

---

## 2. Functional Requirements

### Manual Motion Control

- **REQ-MOT-001:** The system shall provide positive and negative movement controls for X, Y, Z, A, B and C.
- **REQ-MOT-002:** The system shall allow configurable movement step size.
- **REQ-MOT-003:** The system shall allow configurable movement speed where supported.
- **REQ-MOT-004:** The system shall display current position for supported axes.
- **REQ-MOT-005:** The system shall display target position where applicable.
- **REQ-MOT-006:** The system shall prevent commands that exceed configured axis limits.
- **REQ-MOT-007:** The system shall provide start/stop controls for supported movement workflows.

### Controller Communication

- **REQ-COM-001:** The application shall communicate with the existing controller through the agreed protocol.
- **REQ-COM-002:** The communication implementation shall be isolated behind a hardware abstraction interface.
- **REQ-COM-003:** The system shall display controller connection status.
- **REQ-COM-004:** The system shall detect communication timeout/failure where supported.
- **REQ-COM-005:** The system shall process controller acknowledgements and errors where available.
- **REQ-COM-006:** Protocol-specific values shall not be hardcoded into UI components.

### Robot State

- **REQ-STATE-001:** The application shall maintain a centralized robot state.
- **REQ-STATE-002:** UI, 3D visualization and diagnostics shall consume the centralized state.
- **REQ-STATE-003:** The application shall represent major machine states explicitly.
- **REQ-STATE-004:** State transitions shall be logged where appropriate.

### 3D Visualization

- **REQ-3D-001:** The application shall provide an interactive 3D viewport.
- **REQ-3D-002:** The viewport shall display the supplied mechanical model.
- **REQ-3D-003:** The viewport shall provide coordinate axes and a reference grid.
- **REQ-3D-004:** The viewport shall display current and/or target robot state.
- **REQ-3D-005:** The viewport shall provide basic movement/path visualization.

### G-Code

- **REQ-GCODE-001:** The application shall allow G-code file selection/upload.
- **REQ-GCODE-002:** The application shall parse the agreed G-code command set.
- **REQ-GCODE-003:** The application shall validate supported G-code commands.
- **REQ-GCODE-004:** The application shall provide path preview where feasible.
- **REQ-GCODE-005:** The application shall execute validated commands through the motion/control pipeline.
- **REQ-GCODE-006:** The application shall display execution progress.
- **REQ-GCODE-007:** The application shall support pause, resume and stop where supported by the controller.
- **REQ-GCODE-008:** Unsupported commands shall be reported rather than silently ignored.

### Motion Management

- **REQ-MOTION-001:** The application shall maintain a command queue.
- **REQ-MOTION-002:** Manual movement and G-code execution shall use a common command/control pipeline where practical.
- **REQ-MOTION-003:** The application shall support basic interpolation required by the agreed movement workflow.
- **REQ-MOTION-004:** Motion commands shall be validated before transmission.

### Configuration

- **REQ-CONFIG-001:** Axis limits shall be configurable.
- **REQ-CONFIG-002:** Movement step and speed parameters shall be configurable where supported.
- **REQ-CONFIG-003:** Communication settings shall be configurable.
- **REQ-CONFIG-004:** Machine-specific settings shall not be embedded in UI code.
- **REQ-CONFIG-005:** Configuration shall be validated during startup.

### Diagnostics

- **REQ-DIAG-001:** The application shall log important application and controller events.
- **REQ-DIAG-002:** The application shall report communication failures.
- **REQ-DIAG-003:** The application shall report invalid movement commands.
- **REQ-DIAG-004:** The application shall report G-code validation failures.
- **REQ-DIAG-005:** The application shall provide basic diagnostic information.

### RTOS / Controller Integration

- **REQ-RTOS-001:** The application shall integrate with the existing RTOS/controller interface where documentation is available.
- **REQ-RTOS-002:** Existing controller status and telemetry shall be displayed where exposed by the interface.
- **REQ-RTOS-003:** RTOS firmware development is outside the desktop application implementation unless separately agreed.

### SLAM Integration

- **REQ-SLAM-001:** The application shall integrate with an existing SLAM interface where available.
- **REQ-SLAM-002:** Relevant localization/mapping/status information shall be exposed in the application where agreed.
- **REQ-SLAM-003:** Development of a new SLAM algorithm is not part of the desktop application implementation.

### Deployment

- **REQ-DEP-001:** The application shall be deployable on Ubuntu Linux.
- **REQ-DEP-002:** The application shall be deployable on Raspberry Pi OS.
- **REQ-DEP-003:** Installation and basic configuration instructions shall be provided.

---

## 3. Non-Functional Requirements

### Maintainability
The application should use modular layers and hardware adapters.

### Performance
The UI must remain responsive during normal hardware communication, visualization and G-code processing.

### Reliability
Communication failures and controller errors must be handled explicitly.

### Portability
The application should avoid unnecessary x86-only dependencies.

### Testability
Core validation, parsing, state-management and motion logic should be testable without physical hardware where practical.

### Observability
Important state transitions, communication events and failures should be logged.

---

## 4. Open Technical Inputs

The following remain TBD until the project owner supplies the required information:

- Exact controller
- Communication protocol
- Axis units
- Axis limits
- Coordinate origin
- Coordinate frames
- Robot kinematic model
- CAD/3D model
- G-code dialect/command set
- RTOS interface
- SLAM framework/API
- Controller feedback frequency
- Controller acknowledgement behavior

---

## 5. Requirement Change Policy

New requirements discovered after implementation has started should be added to this document with a unique requirement ID and reviewed for timeline and cost impact before implementation.
