# Firmware

MCU firmware. Sketches later live in `/firmware`. Split **bring-up** vs **production** per [ADR-0002](../decisions/0002-motion-stack.md).

## Responsibilities (production MCU)

- Gait / inverse kinematics loop at **50–100 Hz** (faster if the MCU allows).
- Servo comms (TTL bus) or Phase-1 PWM via PCA9685.
- IMU read and simple attitude hold in stand.
- Parse Pi serial commands; publish telemetry (joints, current, faults).
- **E-stop pin** and connection-loss failsafe (sit or damp; never keep walking).
- Current / temperature limits when the servo protocol supports them.

The Pi owns vision, audio, and high-level modes. The MCU owns real-time motion.

## Phase 1 — Uno + PCA9685 + MG995 (bring-up only)

Goals:

- Map 16 PCA9685 channels; use 12 for a future 12-DOF numbering scheme (FR/FL/RR/RL × coxa/femur/tibia).
- Confirm pulse range per servo without binding.
- Prove external BEC wiring; **never** power servos from the Uno 5 V pin.

Non-goals: trot, IMU fusion, talking to Hailo.

I2C: Uno is 5 V. If the Pi ever drives the PCA9685 directly, use a level shifter. Prefer USB-serial to the Uno so the AI HAT+ can stay on the Pi header.

## Production — bus servos + 32-bit MCU

- Assign servo IDs 1–12 (plus spare) and store them in firmware config, not magic numbers in gait math.
- Command from Pi: mode enum, vx/vy/yaw rate, optional pose.
- Telemetry: position, load/current, voltage, temp, fault bits.
- Watchdog on serial: timeout → safe pose.

Candidates: ESP32-S3 or Teensy 4.1. Lock SKU in a follow-up ADR after a blink-and-bus test.

## Protocol sketch (to be frozen in Phase 5)

Binary framed packets, little-endian, CRC16. Example command payload (not implemented yet):

- `mode` (idle / stand / walk / estop)
- `vx, vy, yaw_rate` (float32 or Q-format)
- `flags`

Do not implement this in Phase 0; this paragraph is the contract for later agents.

## Safety hooks

See [safety](../safety/README.md). No walk on boot. No walk without limits. Hardware e-stop overrides software.
