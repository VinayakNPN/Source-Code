# ADR-001: Use PySide6 for Desktop UI

## Status

Accepted

## Context

The application requires a native Linux desktop interface with multiple interactive controls, real-time status updates and integration with a 3D visualization component.

## Decision

Use **PySide6 with Qt 6** for the desktop user interface.

## Alternatives Considered

- Electron
- GTK
- Tkinter
- Web-based desktop wrapper

## Rationale

PySide6 provides:

- Native desktop widgets
- Mature event-driven architecture
- Signals and slots
- Threading support
- Linux compatibility
- Good Python integration
- Suitable support for complex control interfaces

## Consequences

The application becomes dependent on Qt/PySide6, but gains a mature desktop UI framework suitable for the target platforms.
