# Overview

## Goals (v1)

A robot dog that:

1. **Moves** — 12-DOF quadruped gait (walk / stand / sit), commanded from the Pi.
2. **Sees** — CSI camera + Hailo-8L on the AI HAT+ for real-time detection / tracking.
3. **Hears** — microphone in, optional speaker out (wake word and simple commands).

The body is built by the project owner. v1 layout is frozen in [ADR-0005](../decisions/0005-mechanical-layout.md). Public CAD license is still TBD ([ADR-0003](../decisions/0003-license-tbd.md)).

## Constraints

- Brain: **Raspberry Pi 5, 8 GB**, plus **official AI HAT+ (26 TOPS)**.
- Extra MCU allowed when the Pi cannot meet real-time servo timing or HAT stacking blocks a PWM board.
- Parts already owned (Uno, PCA9685, **12× MG995**) plus the [ADR-0004](../decisions/0004-hardware-freeze.md) buy list. ST3215/clones and ₹1,200 DIY joints are **rejected**.
- Work happens in this git repo; planning docs are the instructions for later AI sessions (`AGENTS.md`).

## Non-goals for v1

- Boston Dynamics–class dynamic parkour or manipulation arms.
- Running large LLMs on-device as the motion controller.
- Copying another project's CAD as the production body.
- ROS 2 as a hard requirement (it is optional later).
- Untethered all-day runtime (battery is a later optimization).

## Frozen hardware (ADR-0004)

| Role | Choice |
| --- | --- |
| Compute | Pi 5 8 GB + AI HAT+ |
| Vision | Official Camera Module 3 |
| Hear | USB mic |
| Gait MCU | ESP32-S3 N16R8 |
| Servos | 12× MG995 + PCA9685 |
| Body IMU | BNO055 |
| Current | ACS712 30A on 6 V |
| Not buying | ST3215/clones, DIY ₹1,200 joints, Teensy (unless later pots) |

Details: [ADR-0004](../decisions/0004-hardware-freeze.md), [hardware](../hardware/README.md), [BOM](../../bom/README.md).
