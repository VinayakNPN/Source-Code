# 10-Week Development Plan

## Project

**Robotic Control & Visualization Software**

**Developer:** Vinayak  
**Project Owner:** Aditya Sir  
**Budget:** ₹35,000  
**Start:** 16 August 2026  
**Target Completion:** 25 October 2026

---

## Week 0 — 16 August 2026

### Phase
Project kickoff & technical discovery

### Tasks

- Confirm hardware model.
- Confirm controller/MCU.
- Obtain communication protocol.
- Review existing firmware/RTOS interface.
- Confirm coordinate system.
- Confirm axis limits.
- Confirm units.
- Obtain CAD/3D model status.
- Confirm G-code expectations.
- Confirm SLAM/ROS2 interface if applicable.
- Finalize initial architecture.

### Client Inputs Required

Hardware documentation, protocol, robot specifications, available APIs and development hardware.

### Deliverable

Technical baseline and implementation plan.

### Payment

**₹10,500**

---

## Week 1 — 17–23 August 2026

### Phase
Application foundation

### Tasks

- Initialize repository.
- Configure Python project.
- Configure PySide6.
- Create application shell.
- Create navigation structure.
- Add configuration manager.
- Add logging.
- Create hardware abstraction interface.
- Create basic robot-state model.
- Establish test framework.

### Deliverable

Runnable desktop application foundation.

---

## Week 2 — 24–30 August 2026

### Phase
Manual control

### Tasks

- Implement X/Y/Z controls.
- Implement A/B/C controls.
- Implement directional controls.
- Add step-size configuration.
- Add speed configuration.
- Add position display.
- Add target position.
- Add axis-limit validation.
- Add basic start/stop behavior.

### Client Confirmation

Coordinate definitions, units, limits and direction conventions.

### Deliverable

Manual-control interface ready for controller integration.

---

## Week 3 — 31 August–6 September 2026

### Phase
Hardware communication

### Tasks

- Implement selected hardware adapter.
- Implement controller connection.
- Implement command encoding.
- Implement command transmission.
- Implement feedback decoding.
- Implement connection status.
- Implement timeout handling.
- Implement basic acknowledgement/error handling.

### Client Dependency

Working hardware and finalized controller protocol.

### Deliverable

Linux application communicating with the controller.

### Payment

**₹8,750**

---

## Week 4 — 7–13 September 2026

### Phase
Control engine

### Tasks

- Implement command manager.
- Implement command queue.
- Implement robot state machine.
- Implement axis validation.
- Implement basic interpolation.
- Implement controller acknowledgement handling.
- Integrate manual movement with the control engine.

### Deliverable

Structured motion-control pipeline.

---

## Week 5 — 14–20 September 2026

### Phase
Feedback, reliability & diagnostics

### Tasks

- Process position feedback.
- Process machine status.
- Add communication monitoring.
- Add timeout handling.
- Add reconnection where supported.
- Add fault states.
- Add diagnostic logs.
- Add basic diagnostic view.

### Deliverable

Closed-loop monitoring and diagnostics.

---

## Week 6 — 21–27 September 2026

### Phase
3D visualization

### Tasks

- Import supplied robot model.
- Create PyVista/VTK viewport.
- Add camera controls.
- Add coordinate axes.
- Add grid.
- Connect robot state to model transformation.
- Display current position.
- Display target position.
- Implement basic movement/path visualization.

### Client Inputs Required

CAD/3D model, joint hierarchy, coordinate frames and kinematic information where available.

### Deliverable

Integrated manual control + hardware feedback + 3D visualization.

### Payment

**₹8,750**

---

## Week 7 — 28 September–4 October 2026

### Phase
G-code

### Tasks

- Implement file loading.
- Implement parser.
- Define internal command representation.
- Implement validator.
- Implement supported command set.
- Implement path generation.
- Implement preview.
- Connect validated commands to command queue.

### Client Inputs Required

G-code specification and representative files.

### Deliverable

Validated G-code workflow ready for execution.

---

## Week 8 — 5–11 October 2026

### Phase
Execution & external integrations

### Tasks

- Implement execution progress.
- Implement pause.
- Implement resume.
- Implement stop.
- Display current command/line.
- Integrate RTOS/controller status.
- Integrate SLAM data where an existing interface is available.
- Complete primary UI workflow.

### Deliverable

Core feature-complete application.

### Payment

**₹3,500**

---

## Week 9 — 12–18 October 2026

### Phase
System testing & optimization

### Tasks

- Manual-control testing.
- Axis-limit testing.
- Communication-failure testing.
- Controller restart testing.
- G-code testing.
- Pause/resume testing.
- Stop testing.
- Long-running test.
- Ubuntu deployment testing.
- Raspberry Pi deployment testing.
- UI/UX refinement.
- Performance optimization.
- Bug fixing.

### Client Involvement

Physical hardware test scenarios and expected behavior.

### Deliverable

Release candidate.

---

## Week 10 — 19–25 October 2026

### Phase
Deployment & handover

### Tasks

- Prepare final Ubuntu build.
- Prepare Raspberry Pi deployment.
- Final configuration.
- Final testing.
- Documentation.
- Installation guide.
- User guide.
- Source-code cleanup.
- Final source-code handover.
- Acceptance review.

### Deliverable

Final project release.

### Payment

**₹3,500**

### Target Completion

**25 October 2026**

---

## Payment Summary

| Date | Milestone | Payment |
|---|---|---:|
| 16 Aug 2026 | Project initiation | ₹10,500 |
| 6 Sep 2026 | Hardware communication | ₹8,750 |
| 27 Sep 2026 | 3D + integrated control | ₹8,750 |
| 11 Oct 2026 | Core feature completion | ₹3,500 |
| 25 Oct 2026 | Final deployment & handover | ₹3,500 |
| **Total** | | **₹35,000** |

---

## Weekly Definition of Done

A weekly milestone is considered complete when:

- Planned implementation is committed.
- Relevant tests are passing.
- No known critical regression remains.
- Required client input for the next dependent phase is available.
- Documentation is updated where architecture/configuration changed.
- The milestone can be demonstrated where practical.
