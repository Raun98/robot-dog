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

Parts already on the bench (Arduino Uno, PCA9685, MG995, and similar) are **prototype / learning** gear. They are listed in the BOM with status `owned`. A walkable dog is expected to use **bus servos** and a stronger motion MCU; that path is `recommended` in the BOM and recorded in [ADR-0002](docs/decisions/0002-motion-stack.md).

Compute is **fixed**: Pi 5 8 GB + AI HAT+. License (open vs closed design) is **TBD** ([ADR-0003](docs/decisions/0003-license-tbd.md)).

## Out of scope right now

No 3D models, no Arduino sketches, no Hailo install scripts, and no purchase orders in this pass.
