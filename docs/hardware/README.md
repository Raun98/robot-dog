# Hardware

Frozen in [ADR-0004](../decisions/0004-hardware-freeze.md). Planning only; pin maps later in `/hardware`.

## Compute (fixed)

[ADR-0001](../decisions/0001-compute.md): Pi 5 8 GB + AI HAT+ 26 TOPS. **Buy if missing:** active cooler, 27W 5V/5A PSU, A2 microSD. Do not run Hailo on a phone charger.

## HAT stacking

AI HAT+ owns the 40-pin + PCIe. **No** second HAT. MCU is USB-serial. PCA9685 is a flying I2C board, not stacked on the Pi.

## Camera (pay extra now)

**Official Camera Module 3** on CAM1. USB or clone CSI is a Hailo dead end.

## Audio

USB mic (required for “hear”). USB speaker optional. No analog jack on Pi 5.

## Motion (frozen)

| Item | Role |
| --- | --- |
| 12× MG995 | Legs. Open-loop. Owned. |
| PCA9685 | 12 PWM channels; 12–15 spare |
| ESP32-S3 N16R8 | Gait MCU; native USB to Pi |
| Uno | Phase 1 PWM jig only |
| I2C level shifter | 3.3 V ESP32 ↔ 5 V PCA9685 |
| BNO055 | Body fusion IMU (not a joint encoder) |
| ACS712 30A | Servo-rail stall detect |
| ATX 5 V (owned) | Tethered servo brick; e-stop cuts this |
| 6 V BEC 20–30 A class | Deferred mobile rail |
| Fuse + e-stop | Hardware |

Never power servos from Uno or Pi 5 V.

## Rejected (do not buy)

- ST3215 / STS3215 **and ~₹2k clones** (~₹24k for twelve).
- DIY printed joints at ₹1,200 ([diy-actuators](../research/diy-actuators.md)).
- Teensy 4.1 unless analog joint pots are a later ADR; use ADS1115 on the ESP32 I2C bus instead.

## MCU

| Board | Status |
| --- | --- |
| Uno | Bring-up |
| ESP32-S3 N16R8 | **Frozen** gait CPU |
| Teensy 4.1 | Optional later |
| Pi 5 | Not the servo loop |

## Mechanical

CAD around **MG995** ([ADR-0005](../decisions/0005-mechanical-layout.md)). Leave volume for Pi + HAT + cooler, CSI ribbon, USB to ESP32, PCA9685, twelve servo leads, ATX harness now / 6 V BEC later. Joint IDs: [channel-map.md](channel-map.md).
