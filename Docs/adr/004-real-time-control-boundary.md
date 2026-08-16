# ADR-004: Keep Deterministic Motion Control in the Controller

## Status

Accepted

## Context

The project requires real-time communication and physical movement control.

## Decision

The Linux application will provide high-level commands, visualization and supervision. Deterministic low-level motor control will remain in the existing MCU/RTOS/controller.

## Rationale

A general-purpose Linux desktop process and Python runtime are not appropriate places to assume hard deterministic motor-control timing.

The controller is the appropriate layer for:

- Motor timing
- Low-level control loops
- Encoder processing
- Hardware safety behavior
- Deterministic motion execution

## Consequences

The desktop application must communicate through a well-defined controller protocol and consume controller feedback.
