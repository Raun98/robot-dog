# ADR-0002: Motion stack (bench vs production)

- **Status:** Accepted
- **Date:** 2026-09-04

## Context

The bench already has an Arduino Uno, a PCA9685 16-channel PWM driver, and MG995-class analog servos. Those parts are useful for learning PWM, power, and channel maps. They are a poor fit for a 12-DOF walking dog: no joint feedback, high stall current, Uno RAM/CPU limits, and the AI HAT+ blocking a second PWM HAT on the Pi.

## Decision

**Phase 1 (bench, owned parts):**

- Uno + PCA9685 + a few MG995s to prove power rails, pulse ranges, and mechanical horns.
- This stack is **not** the walking production controller.

**Production (recommended):**

- **Actuators:** Feetech-style TTL bus servos (STS3215 class or equivalent) with position, current, and temperature telemetry.
- **Drive:** UART/TTL bus (daisy-chain). PCA9685 is **not** required on this path.
- **MCU:** ESP32-S3 or Teensy 4.1 (or a vendor bus-servo adapter). Uno remains a test jig only.
- **Link to Pi:** USB-serial (preferred) so the AI HAT+ can keep the 40-pin header.

A later ADR may lock one MCU and one exact servo SKU after a short bake-off.

## Consequences

- BOM lists Uno / PCA9685 / MG995 as `owned` and bus servos / production MCU as `recommended`.
- Firmware is split: bring-up sketches vs gait firmware. See `docs/firmware/`.
- Mechanical mounts must not assume MG995 horn geometry as final if production servos differ.

## Alternatives considered

- Pi GPIO bit-bang or pigpio for 12 servos: rejected (timing, Linux jitter, HAT pin conflicts).
- Uno as permanent gait CPU: rejected (too little compute/RAM, no native USB-serial convenience vs 32-bit boards, 5 V I2C to Pi needs level shifting).
- Keep MG995 for walking: rejected for feedback and durability; allowed only for static bench and maybe a non-walking mockup.
