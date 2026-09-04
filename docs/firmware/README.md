# Firmware

MCU firmware. Sketches later live in `/firmware`. Split **bring-up** vs **v1 gait** per [ADR-0002](../decisions/0002-motion-stack.md).

## Responsibilities (gait MCU)

- Gait / inverse kinematics loop at **50–100 Hz** (faster if the MCU allows).
- v1 servo comms: PCA9685 PWM, 12 channels, MG995.
- IMU read and simple attitude hold in stand (once a 32-bit MCU is in).
- Parse Pi serial commands; publish telemetry (MCU faults; no true joint encoders on MG995).
- **E-stop pin** and connection-loss failsafe (sit or damp; never keep walking).
- Keep pulse widths inside calibrated, non-binding ranges.

The Pi owns vision, audio, and high-level modes. The MCU owns real-time motion.

## Phase 1 — Uno + PCA9685 + MG995 (bring-up)

Goals:

- Map 16 PCA9685 channels; use 12 for 12-DOF numbering (FR/FL/RR/RL × coxa/femur/tibia).
- Confirm pulse range per servo without binding.
- Prove external BEC wiring; **never** power servos from the Uno 5 V pin.

Non-goals: full trot on the floor, IMU fusion, talking to Hailo.

I2C: Uno is 5 V. If a 3.3 V MCU or the Pi drives the PCA9685, use a level shifter. Prefer USB-serial to the MCU so the AI HAT+ can stay on the Pi header.

## v1 gait — 32-bit MCU + same PCA9685

- Keep channel numbers in firmware config, not magic numbers in gait math.
- Command from Pi: mode enum, vx/vy/yaw rate, optional pose.
- Watchdog on serial: timeout → safe pose.
- Candidates: **ESP32-S3** (recommended). Teensy 4.1 optional. Still I2C to PCA9685.
- IMU: MPU-6050 on the gait MCU (complementary filter).

## Later — bus servos (deferred)

Only if MG995 walking is not good enough. Then: ST3215/STS3215 IDs 1–12, drop PCA9685, current/temp telemetry. New ADR + BOM change.

## Protocol sketch (to be frozen in Phase 5)

Binary framed packets, little-endian, CRC16. Example command payload (not implemented yet):

- `mode` (idle / stand / walk / estop)
- `vx, vy, yaw_rate` (float32 or Q-format)
- `flags`

Do not implement this in Phase 0; this paragraph is the contract for later agents.

## Safety hooks

See [safety](../safety/README.md). No walk on boot. Conservative PWM. Hardware e-stop overrides software.
