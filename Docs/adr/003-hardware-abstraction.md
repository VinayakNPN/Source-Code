# ADR-003: Hardware Abstraction Layer

## Status

Accepted

## Context

The exact controller communication protocol is not finalized at project kickoff.

## Decision

Introduce a hardware abstraction interface between the control engine and physical communication implementation.

## Rationale

This allows the application to support the required controller without coupling UI or motion logic to a specific protocol.

Potential adapters:

```text
HardwareInterface
├── SerialAdapter
├── TCPAdapter
├── CANAdapter
└── ROS2Adapter
```

Only the required adapter should be implemented initially.

## Consequences

Additional interface code is required, but the application becomes easier to test and adapt.
