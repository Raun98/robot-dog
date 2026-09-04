# Overview

## Goals (v1)

A robot dog that:

1. **Moves** — 12-DOF quadruped gait (walk / stand / sit), commanded from the Pi.
2. **Sees** — CSI camera + Hailo-8L on the AI HAT+ for real-time detection / tracking.
3. **Hears** — microphone in, optional speaker out (wake word and simple commands).

The body is built by the project owner. Mechanical design may be released later; that choice is not made yet.

## Constraints

- Brain: **Raspberry Pi 5, 8 GB**, plus **official AI HAT+ (26 TOPS)**.
- Extra MCU allowed when the Pi cannot meet real-time servo timing or HAT stacking blocks a PWM board.
- Parts already owned (Uno, PCA9685, MG995, etc.) may be used for learning and may be **replaced** if they cannot walk a dog safely.
- Work happens in this git repo; planning docs are the instructions for later AI sessions (`AGENTS.md`).

## Non-goals for v1

- Boston Dynamics–class dynamic parkour or manipulation arms.
- Running large LLMs on-device as the motion controller.
- Copying another project's CAD as the production body.
- ROS 2 as a hard requirement (it is optional later).
- Untethered all-day runtime (battery is a later optimization).

## Owned hardware vs production path

| Role | On the bench now | Intended for a walking dog |
| --- | --- | --- |
| Compute | Pi 5 8 GB + AI HAT+ | Same (fixed) |
| Motion MCU | Arduino Uno | ESP32-S3, Teensy 4.1, or bus-servo adapter |
| Servo drive | PCA9685 PWM | TTL/UART bus (e.g. STS3215 class) |
| Actuators | MG995 analog | Smart bus servos with position/current feedback |

Details: [ADR-0002](../decisions/0002-motion-stack.md), [hardware](../hardware/README.md), [BOM](../../bom/README.md).
