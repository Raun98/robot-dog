# Electrical work by build phase

Circuits follow [build phases](../build-phases/README.md). Do not build the full PDB in Phase 1.

## Phase 0 — now (planning)

- Dual-rail topology, ratings, grounding, e-stop philosophy: this folder.
- Electrical SKUs only in [`bom/bom.csv`](../../bom/bom.csv).
- No requirement for KiCad or a physical PDB.

**Exit for EE:** an agent can wire Phase 1 from these notes without changing ADR-0004.

## Phase 1 — motion bench (owned + BEC)

**Goal:** 1–2 MG995 on PCA9685, Uno, external 6 V, kill switch.

| Include | Exclude |
| --- | --- |
| External BEC or bench PSU on servo V+ | Pi 5 / AI HAT+ on the servo supply |
| Series kill switch in servo V+ | Software-only stop |
| PCA9685 VCC from Uno 5 V (logic) | Servo current from Uno 5 V pin |
| Common GND: Uno, PCA9685, BEC, servos | ACS712, level shifter, ESP32 (optional later in this phase) |

Bench PSU: current-limit ~3–5 A for two MG995. Watch for stall (horn jammed).

## Phase 2 — vision (compute rail only)

Tethered **27 W-class** 5 V / 5 A into the Pi. Servo rail **off** or disconnected. Camera CSI is not an EE redesign; strain-relieve the ribbon.

## Phase 3 — audio

USB mic/speaker draw is small versus Hailo. Stay on the Pi 5 V rail. No analog headphone path on Pi 5.

## Phase 4 — twelve servos on a stand

Build the PDB in [power-distribution.md](power-distribution.md):

- Pack or high-current 6 V source → fuse → e-stop high-side → PCA9685 V+ and servo leads
- ACS712 in the servo high-side path
- ESP32-S3 USB to Pi; I2C via level shifter
- Prove e-stop **before** stand-pose under load

## Phase 5 — walk

Same hardware as Phase 4. EE job is ratings and failsafe, not gait. Confirm USB cable is not the servo return.

## Later — battery (PWR-002 / PWR-003)

2S/3S pack + 6 V BEC for MG995, plus a **≥27 W** 5 V buck for Pi + AI HAT+. Size the pack after measured walk current. Do not buy a 12 V bus-servo pack while ST3215 is rejected.
