# Electrical work by build phase

Circuits follow [build phases](../build-phases/README.md). Do not build the full twelve-servo harness in Phase 1.

## Phase 0 — now (planning)

- Dual-rail topology, ratings, grounding, e-stop philosophy: this folder.
- Electrical SKUs only in [`bom/bom.csv`](../../bom/bom.csv).
- No requirement for KiCad or a physical PDB.

**Exit for EE:** an agent can wire Phase 1 from these notes without changing ADR-0004.

## Phase 1 — motion bench (owned ATX)

**Goal:** 1–2 MG995 on PCA9685, Uno, fused ATX 5 V, kill switch or e-stop.

| Include | Exclude |
| --- | --- |
| ATX 5 V red, fused, to servo positives | Pi 5 / AI HAT+ on the servo supply; ATX 5 V into the Pi |
| Series kill switch or e-stop in servo V+ | Software-only stop |
| PCA9685 VCC from Uno 5 V (logic) | Servo current from Uno 5 V pin or PCA9685 V+ |
| Common GND: Uno, PCA9685, ATX COM, servos | ACS712, level shifter, ESP32 (optional later in this phase) |

Two MG995: a 5 A-class limit is enough if the ATX is current-limited or you watch for stall (horn jammed). Confirm PS_ON is tied to GND so the ATX stays on.

## Phase 2 — vision (compute rail only)

Tethered **27 W-class** 5 V / 5 A USB-C into the Pi. Servo rail **off** or disconnected. Camera CSI is not an EE redesign; strain-relieve the ribbon.

## Phase 3 — audio

USB mic/speaker draw is small versus Hailo. Stay on the Pi 5 V rail. No analog headphone path on Pi 5.

## Phase 4 — twelve servos on a stand

Build the bus in [power-distribution.md](power-distribution.md):

- ATX 5 V (if **+5 V ≥ ~25 A**) → ATO 30 A → e-stop high-side → ACS712 → fat bus → twelve MG995
- Do not route stall through PCA9685 V+
- ESP32-S3 USB to Pi; I2C via level shifter
- Prove e-stop **before** stand-pose under load

## Phase 5 — walk

Same hardware as Phase 4. EE job is ratings and failsafe, not gait. Confirm USB cable is not the servo return.

## Later — battery (PWR-001 / PWR-002 / PWR-003)

3S pack + Hobbywing-class 6 V BEC for MG995, plus a **≥27 W** 5 V buck for Pi + AI HAT+. Size the pack after measured walk current. Do not buy a 12 V bus-servo pack while ST3215 is rejected. Do not order the UBEC in W0.
