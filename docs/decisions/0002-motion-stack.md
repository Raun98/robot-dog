# ADR-0002: Motion stack (v1 MG995, bus servos deferred)

- **Status:** Accepted (revised 2026-09-04)
- **Date:** 2026-09-04

## Context

The owner has an Arduino Uno, a PCA9685, and **twelve MG995** analog servos. An earlier draft treated MG995 as bench-only and recommended buying twelve Feetech-style bus servos immediately. That bus family is expensive (~USD 17–22 each, ~USD 200–260 for twelve, plus adapter). The owner chose to **walk on the MG995s first** and buy bus servos only if that is not good enough.

**Naming:** Feetech **STS3215** (OEM) and Waveshare **ST3215** are the same TTL bus-servo family, not two different motors. Docs may say ST3215 / STS3215.

## Decision

**v1 walking actuators (owned):**

- Twelve MG995s, PCA9685 PWM, external **5–6 V BEC sized for stall** (never the Uno 5 V pin).
- Hardware e-stop on the servo rail. First walks on a sling or stand.
- CAD and horns may assume MG995 geometry for v1.

**v1 MCU:**

- Uno is for Phase 1 PWM bring-up only (RAM/CPU, 5 V I2C). It is not the long-term gait + IMU computer.
- **ESP32-S3** (MCU-002, ~₹500–1,000) **driving the same PCA9685** over I2C (level shift as needed) is the preferred gait MCU. That upgrade does **not** require new servos.
- **Teensy 4.1** (MCU-003) stays optional; skip on an INR-tight budget.
- Link to Pi: USB-serial so the AI HAT+ keeps the 40-pin header. Do not stack a PWM HAT on the Pi.

**v1 IMU / current sense:** superseded by [ADR-0004](0004-hardware-freeze.md) (**BNO055**, ACS712 **30A**).

**Deferred / rejected:**

- ST3215 / clones: **rejected** in [ADR-0004](0004-hardware-freeze.md) (~₹2k each).
- DIY print at ₹1,200: **rejected** for walking. [`diy-actuators.md`](../research/diy-actuators.md).

## Honest limits (not a veto)

MG995s are analog, have no joint feedback, draw high stall current, and wear faster than bus servos. Conservative PWM ranges and a fat power rail are mandatory.

## Consequences

- BOM: see ADR-0004. SRV-002 and ACT-001 are `rejected`. IMU-001 is BNO055. MCU-002 is ESP32-S3.
- Firmware: PCA9685 channel map is the v1 servo interface.
- Mechanical: design v1 around MG995; do not wait on a bus-servo body.

## Alternatives considered

- Buy twelve ST3215 now: rejected on cost; owner already has 12 MG995s.
- Pi GPIO bit-bang of 12 servos: rejected (Linux jitter, HAT pin conflict).
- Uno as permanent gait CPU: rejected for compute; keep as jig.
- MG995 never used for walking: superseded by this revision.
- Replace MG995s with ₹1,200 3D-printed actuators: rejected; not walkable (see diy-actuators study).
