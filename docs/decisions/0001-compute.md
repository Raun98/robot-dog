# ADR-0001: Compute platform

- **Status:** Accepted
- **Date:** 2026-09-04

## Context

The dog needs on-board vision (and later other neural nets) without a laptop in the loop. The owner already standardizes on Raspberry Pi 5 8 GB and the official 26 TOPS AI HAT+.

## Decision

- **Brain:** Raspberry Pi 5, 8 GB RAM.
- **Accelerator:** Raspberry Pi AI HAT+ (Hailo-8L class, 26 TOPS).
- **OS (planned):** Raspberry Pi OS 64-bit; install Hailo via `hailo-all`; enable PCIe Gen 3 for best throughput.
- **Camera:** Official CSI camera (Camera Module 3 preferred) on CAM1 for Hailo + `rpicam` demos.

## Consequences

- GPIO HAT stacking for a PCA9685 (or similar) on the same 40-pin header is **not** the production wiring plan. See hardware docs.
- Cooling: Pi 5 active cooler is expected; Hailo also needs airflow.
- Power budget is dominated by Pi + HAT + camera, separate from servo current. See electrical docs.

## Alternatives considered

- Pi 4 + USB Coral / Hailo M.2 elsewhere: weaker CPU and messier I/O; rejected because Pi 5 + AI HAT+ is already chosen.
- Laptop off-board inference: rejected for a self-contained dog.
