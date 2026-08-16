# Hardware Interface Specification

> Status: TEMPLATE — to be finalized after controller documentation and hardware access are provided.

## 1. Purpose

This document defines the communication contract between the Linux application and the existing robotic controller.

The application should communicate through a hardware abstraction interface so that protocol-specific implementation remains isolated.

```text
Linux Application
      │
      ▼
Hardware Interface
      │
      ▼
Controller Protocol
      │
      ▼
Existing Controller / RTOS
      │
      ▼
Mechanical System
```

## 2. Controller Information

| Parameter | Value |
|---|---|
| Controller | TBD |
| MCU | TBD |
| Firmware | TBD |
| Protocol | TBD |
| Physical interface | TBD |
| Baud rate | TBD |
| IP address | TBD |
| Port | TBD |
| CAN configuration | TBD |
| ROS2 interface | TBD |

## 3. Connection Lifecycle

Expected lifecycle:

```text
DISCONNECTED
     ↓
CONNECTING
     ↓
CONNECTED
     ↓
READY
     ↓
DISCONNECTED / FAULT
```

Exact states depend on controller capabilities.

## 4. Command Contract

To be completed from controller documentation.

| Command | Direction | Parameters | Response | Notes |
|---|---|---|---|---|
| MOVE | App → Controller | TBD | TBD | TBD |
| STOP | App → Controller | TBD | TBD | TBD |
| PAUSE | App → Controller | TBD | TBD | TBD |
| RESUME | App → Controller | TBD | TBD | TBD |
| STATUS | App → Controller | TBD | TBD | TBD |

## 5. Feedback Contract

| Feedback | Source | Unit | Frequency | Required |
|---|---|---|---|---|
| X position | TBD | TBD | TBD | TBD |
| Y position | TBD | TBD | TBD | TBD |
| Z position | TBD | TBD | TBD | TBD |
| A position | TBD | TBD | TBD | TBD |
| B position | TBD | TBD | TBD | TBD |
| C position | TBD | TBD | TBD | TBD |
| Machine state | TBD | N/A | TBD | TBD |
| Error state | TBD | N/A | TBD | TBD |

## 6. Error Handling

Document:

- Error code format
- Timeout behavior
- Retry policy
- Reconnection behavior
- Controller fault behavior
- Invalid command behavior
- Emergency-stop interaction

## 7. Heartbeat / Watchdog

TBD after controller protocol review.

Define:

- Heartbeat interval
- Timeout threshold
- Expected response
- Action after timeout

## 8. Coordinate & Unit Specification

| Axis | Unit | Min | Max | Origin | Positive Direction |
|---|---|---:|---:|---|---|
| X | TBD | TBD | TBD | TBD | TBD |
| Y | TBD | TBD | TBD | TBD | TBD |
| Z | TBD | TBD | TBD | TBD | TBD |
| A | TBD | TBD | TBD | TBD | TBD |
| B | TBD | TBD | TBD | TBD | TBD |
| C | TBD | TBD | TBD | TBD | TBD |

## 9. Protocol Sign-Off

**Controller documentation received:** __________

**Reviewed by:** ______________________________

**Date:** _____________________________________
