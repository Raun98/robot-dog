# Hardware

Planning only. Implementation artifacts later go in `/hardware`.

## Compute (fixed)

See [ADR-0001](../decisions/0001-compute.md).

- Raspberry Pi 5 8 GB.
- Official AI HAT+ (26 TOPS, Hailo-8L class) on the Pi 5 PCIe FPC + GPIO stack.
- Pi 5 **active cooler** expected.
- Software bring-up (later): Raspberry Pi OS 64-bit, `sudo apt install hailo-all`, PCIe Gen 3, `hailortcli fw-control identify`, CSI camera on CAM1.

## HAT stacking (hard constraint)

The AI HAT+ sits on the 40-pin header and uses PCIe. **Do not** plan a PCA9685 HAT (or any second HAT) stacked on that same header.

Options that are allowed:

- USB-serial to a motion MCU (preferred).
- Flying-lead UART/I2C from remaining pins if a pinout is verified against the AI HAT+ leftover pins.
- USB camera only as camera fallback (CSI preferred).

## Camera

- **Preferred:** Raspberry Pi Camera Module 3 (CSI, CAM1) for Hailo + `rpicam` pipelines.
- Route the ribbon through the body with strain relief; 180° vs standard camera orientation is a CAD item.
- High Quality Camera is optional if a C-mount lens is needed later.

## Audio

Pi 5 has **no analog audio jack**.

- **Hear:** USB microphone, or I2S MEMS mic (ReSpeaker-style USB is simpler on a crowded GPIO).
- **Speak (optional v1):** USB speaker or I2S DAC + small speaker.
- Keep USB devices on a powered hub if the Pi port current is tight once Wi-Fi + Hailo + camera are running.

## Motion — v1 (owned, walk on these)

See [ADR-0002](../decisions/0002-motion-stack.md).

| Item | Role |
| --- | --- |
| Arduino Uno | PWM/I2C bring-up MCU; 5 V logic; not the long-term gait CPU |
| PCA9685 | 16-ch I2C PWM; **12 channels** for 12-DOF walk |
| MG995 × 12 | v1 walking actuators; analog; no position feedback; high stall current |

Use a **stall-rated** 5–6 V BEC for all twelve, not a tiny hobby BEC meant for one or two servos. Never power MG995s from the Uno 5 V pin.

A later ESP32-S3 (or similar) should talk to this **same PCA9685** over I2C (level shifter). That does not require new servos.

## Motion — deferred (only if MG995 walk fails)

| Item | Role |
| --- | --- |
| Waveshare **ST3215** / Feetech **STS3215** (same family) | TTL bus, encoder, current/temp; ~USD 17–22 each so twelve is ~USD 200–260+ |
| Bus adapter | Replaces PCA9685 on that path |
| IMU (e.g. ICM-42688 / BNO085 class) | Attitude for stand/walk (useful on v1 MCU too) |

Confirm 7.4 V vs 12 V SKU before any purchase. Do not buy a set until v1 walking shows MG995s are the limiter.

## MCU comparison (short)

| Board | Keep? | Why |
| --- | --- | --- |
| Arduino Uno | Bench only | 16 MHz, 2 KB SRAM; OK for PWM sweep, weak for gait + IMU |
| ESP32-S3 | Recommended candidate | Gait + IMU while still driving PCA9685 |
| Teensy 4.1 | Recommended candidate | Strong real-time; more cost |
| Pi 5 alone | Not for servo loop | Linux jitter; HAT pin conflict |

## Mechanical-electrical interface

Leave volume and airflow for: Pi 5 + HAT + cooler, battery, CSI ribbon, USB to MCU, PCA9685, and twelve MG995 power/signal leads. Mass budget: see [design](../design/README.md).
