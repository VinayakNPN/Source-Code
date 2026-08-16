# Deployment Guide

> Status: Initial template. Final instructions depend on the selected packaging strategy and hardware interface.

## 1. Supported Platforms

- Ubuntu Linux
- Raspberry Pi OS

## 2. Development Installation

Create a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Run:

```bash
python -m app.main
```

## 3. Production Deployment

The final project should provide a repeatable deployment method.

Preferred options:

- `.deb` package
- Self-contained application bundle
- Python environment installation

The final approach will be selected after validating Raspberry Pi compatibility.

## 4. Configuration

Machine-specific configuration should be stored separately from source code.

Example:

```text
/etc/robot-control/machine.yaml
```

or an application-specific configuration directory.

## 5. Hardware Permissions

If serial/USB/CAN devices require OS permissions, document the required user/group configuration.

Avoid running the full application as root unless there is a verified requirement.

## 6. Ubuntu Verification

Verify:

- Application starts
- Configuration loads
- 3D renderer works
- Controller connects
- Manual control works
- G-code workflow works
- Logs are generated
- Application shuts down correctly

## 7. Raspberry Pi Verification

Verify:

- Application starts
- Controller interface is available
- 3D rendering performance is acceptable
- Manual control works
- G-code workflow works
- Logs are generated
- No x86-only dependency is required

## 8. Release Checklist

- [ ] Version tagged
- [ ] Dependencies verified
- [ ] Configuration documented
- [ ] Ubuntu build verified
- [ ] Raspberry Pi build verified
- [ ] Hardware connection verified
- [ ] Test plan completed
- [ ] Documentation updated
