# Acceptance Criteria

This document defines measurable completion criteria for the agreed project scope.

## Manual Control

### AC-MOT-001
Given a connected controller, pressing a configured X+ control shall issue the corresponding positive X movement command.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-MOT-002
The application shall reject movement beyond configured axis limits.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-MOT-003
Step size shall be configurable and reflected in manual movement commands.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-MOT-004
The application shall expose X/Y/Z/A/B/C movement controls according to the approved UI.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

## Communication

### AC-COM-001
The application shall connect to the agreed controller interface.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-COM-002
A valid command shall reach the controller using the agreed protocol.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-COM-003
Controller disconnection shall be detected and displayed.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-COM-004
Controller errors/acknowledgements shall be handled according to the agreed protocol.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

## Robot State

### AC-STATE-001
The application shall display the current controller/robot state where supported.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-STATE-002
The 3D view and status UI shall reflect the centralized robot state.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

## 3D Visualization

### AC-3D-001
The supplied robot model shall load successfully.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-3D-002
The viewport shall provide basic camera controls.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-3D-003
Robot position/orientation shall be reflected in the visualization where required data is available.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-3D-004
Movement/path visualization shall be displayed for supported workflows.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

## G-Code

### AC-GCODE-001
A supported G-code file shall load successfully.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-GCODE-002
Invalid or unsupported commands shall be reported.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-GCODE-003
Validated commands shall enter the execution pipeline.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-GCODE-004
Execution progress shall be visible.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-GCODE-005
Pause, resume and stop shall behave according to controller capabilities.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

## Diagnostics

### AC-DIAG-001
Important connection and controller events shall be logged.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-DIAG-002
Communication failure shall generate a useful diagnostic message.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

## Deployment

### AC-DEP-001
The application shall install and start on the agreed Ubuntu environment.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

### AC-DEP-002
The application shall install and start on the agreed Raspberry Pi OS environment.

**Status:** [ ] Not Tested [ ] Passed [ ] Failed

## Final Acceptance

All applicable acceptance criteria must be reviewed before final handover.

**Client approval:** ______________________

**Developer:** ____________________________

**Date:** _________________________________
