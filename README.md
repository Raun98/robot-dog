# Robot dog

A DIY quadruped that **moves**, **sees**, and **hears**, with a Raspberry Pi 5 (8 GB) and Raspberry Pi AI HAT+ (26 TOPS) as the brain.

This repository is in **planning**. There is no walking robot firmware or CAD here yet. Research, architecture, a bill of materials, and agent instructions live under `docs/`, `bom/`, and `AGENTS.md`.

## Current phase

**Phase 0 — documentation.** See [docs/build-phases/](docs/build-phases/README.md).

## How to navigate

| Area | Planning (source of truth) | Future implementation |
| --- | --- | --- |
| Overview | [docs/overview/](docs/overview/README.md) | — |
| Architecture | [docs/architecture/](docs/architecture/README.md) | — |
| Decisions (ADRs) | [docs/decisions/](docs/decisions/README.md) | — |
| Hardware | [docs/hardware/](docs/hardware/README.md) | [hardware/](hardware/README.md) |
| Electrical | [docs/electrical/](docs/electrical/README.md) | — |
| Firmware | [docs/firmware/](docs/firmware/README.md) | [firmware/](firmware/README.md) |
| Software | [docs/software/](docs/software/README.md) | [software/](software/README.md) |
| Mechanical design | [docs/design/](docs/design/README.md) | [design/](design/README.md) |
| Safety | [docs/safety/](docs/safety/README.md) | — |
| Parts list | [bom/](bom/README.md) | — |
| References | [docs/research/references.md](docs/research/references.md) | — |

Human and AI contributors should start with [AGENTS.md](AGENTS.md).

## Owned vs recommended hardware

**Frozen stack (ADR-0004):** 12× MG995 + PCA9685 + ESP32-S3 + BNO055. ST3215/clones (~₹2k each) and DIY joints at ₹1,200 are **rejected**. Spend extra now on Camera Module 3, a stall-rated 6 V BEC, Pi cooler/27W PSU, and BNO055 — not on bus-servo clones.

Compute is **fixed**: Pi 5 8 GB + AI HAT+. License TBD ([ADR-0003](docs/decisions/0003-license-tbd.md)).

## Out of scope right now

No 3D models, no Arduino sketches, no Hailo install scripts, and no purchase orders in this pass.
