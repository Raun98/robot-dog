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
- A 32-bit board (ESP32-S3 candidate) **driving the same PCA9685** over I2C (level shift as needed) is the preferred gait MCU. That upgrade does **not** require new servos.
- Link to Pi: USB-serial so the AI HAT+ keeps the 40-pin header. Do not stack a PWM HAT on the Pi.

**Deferred (buy only if MG995 walk fails):**

- ST3215 / STS3215 (or cheaper bus equivalent) when joints will not hold calibration, overheat, strip, or you need position/current feedback for a stable gait.
- That path drops PCA9685 for a UART TTL bus. Record the buy in a new ADR and the BOM.

## Honest limits (not a veto)

MG995s are analog, have no joint feedback, draw high stall current, and wear faster than bus servos. Conservative PWM ranges and a fat power rail are mandatory.

## Consequences

- BOM: SRV-001 twelve `owned` v1 actuators; SRV-002 ST3215/STS3215 `deferred`.
- Firmware: PCA9685 channel map is the v1 servo interface; bus IDs are later.
- Mechanical: design v1 around MG995; do not wait on a bus-servo body.

## Alternatives considered

- Buy twelve ST3215 now: rejected on cost; owner already has 12 MG995s.
- Pi GPIO bit-bang of 12 servos: rejected (Linux jitter, HAT pin conflict).
- Uno as permanent gait CPU: rejected for compute; keep as jig.
- MG995 never used for walking: superseded by this revision.
