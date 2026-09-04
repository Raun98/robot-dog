# ADR-0004: Frozen v1 hardware stack

- **Status:** Accepted
- **Date:** 2026-09-04

## Context

Phase 0 compared ST3215 / clones (~₹2,000 each, ~₹24k for twelve), DIY printed joints (₹1,200 cap, not walkable), Teensy vs ESP32-S3, and MPU-6050 vs fusion IMUs. The owner ruled **ST3215 clones out on price**. Paying more **now** is allowed when it avoids a dead-end (Hailo camera, power, body IMU).

## Decision — frozen stack

```text
[Camera Module 3 CSI] --> [Pi 5 8GB + AI HAT+ 26 TOPS] --> vision / audio / behavior
[USB mic] --------------^
        USB-serial
            v
[ESP32-S3 N16R8] --I2C (level shift)--> [PCA9685] --> 12x MG995
       |-- I2C --> [BNO055]
       |-- analog --> [ACS712 30A on servo rail]
       |-- GPIO --> e-stop sense
ATX 5V (bench) or 6V stall-rated BEC (mobile) --> MG995 power bus (e-stop cuts this rail)
5V 5A class USB-C --> Pi only (not servos; not ATX 5V)
```

| Layer | Frozen choice | Buy more now? |
| --- | --- | --- |
| Brain | Pi 5 8GB + AI HAT+ (owned) | Cooler + 27W PSU if missing — Hailo needs them |
| Vision | Official **Camera Module 3** CSI | **Yes.** USB/clone CSI is a Hailo dead end |
| Hear | USB microphone | Cheap is fine |
| Speak | USB speaker | Optional |
| Gait MCU | **ESP32-S3 DevKit N16R8** (native USB) | **Yes vs Uno.** Same chip can later talk UART if actuators ever change |
| Bring-up MCU | Uno (owned) | No |
| Servo drive | PCA9685 (owned) | Keep; do not stack as a Pi HAT |
| Legs | **12× MG995** (owned) | No ST3215, no clones, no printed QDD |
| Body IMU | **BNO055** (on-chip fusion) | **Yes vs GY-521 clones.** Best cheap compensation for no joint encoders |
| Current | **ACS712 30A** on the servo rail | Yes (30A, not INA219 3.2A) |
| I2C | Bidirectional **level shifter** 3.3/5 V | Yes (ESP32 + PCA9685 + BNO) |
| Power | Separate Pi 5V vs servo stall-rated **~20–30 A** | **Bench:** owned **ATX 5 V** if label **+5V ≥ ~25 A**. **Do not buy UBEC this cart.** Mobile/6 V = Hobbywing `30606000` later with 3S |
| Safety | Hardware e-stop + fuse on servo rail | Yes |

**Explicitly out (do not buy):**

- ST3215 / STS3215 **and clones** (~₹2k/motor). Reopen only with a new ADR and a real budget.
- DIY 3D-printed walk actuators at ₹1,200 (ACT-001 study only).
- Teensy 4.1 as a required buy. Optional later if analog joint pots are added; until then ESP32 + I2C (BNO055, optional ADS1115) is the upgrade path.
- PWM from the Pi, second HAT on the AI HAT+, INA219 3.2 A, phone-charger Pi PSU under Hailo load.

## Upgrade hooks (without changing v1 SKUs)

- ESP32 UART reserved in firmware docs for a possible future bus; v1 is I2C PCA9685 only.
- Extra PCA9685 channels 12–15 unused (or fan/aux).
- I2C bus: BNO055 + PCA9685; later ADS1115 for a few joint pots without a Teensy.
- Mechanical: MG995 horn/body. A later actuator swap is a new CAD, not a hidden assumption.

## Consequences

- BOM statuses match this table. `rejected` = do not purchase for this project unless a new ADR.
- Agents treat this ADR as the hardware source of truth together with ADR-0001.
