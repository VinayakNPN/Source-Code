# Test Plan

## 1. Objective

Verify that the application meets functional requirements and behaves predictably during normal and failure scenarios.

## 2. Unit Testing

Test:

- Axis-limit validation
- Coordinate validation
- Command generation
- State transitions
- G-code parser
- G-code validator
- Configuration validation
- Motion interpolation
- Protocol encoding/decoding

Target: core control logic should be testable without physical hardware.

## 3. Integration Testing

Test:

- Hardware adapter
- Controller connection
- Command transmission
- Feedback processing
- Command queue
- G-code execution
- Robot state updates

## 4. Hardware Testing

When hardware is available:

### Connection

- Connect
- Disconnect
- Reconnect
- Controller restart

### Motion

- X+
- X-
- Y+
- Y-
- Z+
- Z-
- A+
- A-
- B+
- B-
- C+
- C-

### Boundaries

- Minimum valid position
- Maximum valid position
- Below minimum
- Above maximum

### Failure

- Cable disconnect
- Controller timeout
- Invalid response
- Controller fault
- Application restart

### G-Code

- Valid file
- Empty file
- Invalid syntax
- Unsupported command
- Pause
- Resume
- Stop
- Long-running execution

## 5. 3D Testing

- Model loading
- Camera controls
- Coordinate alignment
- Position updates
- Path rendering
- Large path performance

## 6. Deployment Testing

Test on:

- Ubuntu development machine
- Clean Ubuntu environment
- Raspberry Pi OS
- Target Raspberry Pi hardware

Verify:

- Installation
- Startup
- Configuration
- Hardware access
- 3D rendering
- Logging
- Shutdown

## 7. Regression Testing

Every significant hardware/control change should rerun:

- Unit tests
- Communication tests
- Basic movement tests
- G-code smoke test

## 8. Test Record

| Test ID | Description | Result | Date | Notes |
|---|---|---|---|---|
| TEST-001 | X+ movement | TBD | | |
| TEST-002 | Axis-limit validation | TBD | | |
| TEST-003 | Controller disconnect | TBD | | |
| TEST-004 | G-code load | TBD | | |
| TEST-005 | G-code stop | TBD | | |
| TEST-006 | 3D model load | TBD | | |
| TEST-007 | Ubuntu deployment | TBD | | |
| TEST-008 | Raspberry Pi deployment | TBD | | |
