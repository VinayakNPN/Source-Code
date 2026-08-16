# ADR-002: Use Python for the High-Level Application

## Status

Accepted

## Context

The application requires desktop UI, G-code parsing, configuration, logging, visualization and integration with several possible communication protocols.

## Decision

Use Python for the high-level Linux application.

## Rationale

Python provides strong libraries for:

- PySide6
- Serial communication
- CAN
- Networking
- ROS2 integration
- G-code processing
- Testing
- Configuration
- Visualization

The existing controller/RTOS remains responsible for deterministic low-level control.

## Consequences

Python is not treated as the hard real-time motor-control layer. The architecture must preserve this boundary.
