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

## Motion — bench (owned)

See [ADR-0002](../decisions/0002-motion-stack.md).

| Item | Role |
| --- | --- |
| Arduino Uno | PWM/I2C test MCU; 5 V logic; not the production gait CPU |
| PCA9685 | 16-ch I2C PWM; 12 channels used for a 12-DOF mock; needs 3.3/5 V I2C care if ever talked to from the Pi |
| MG995 | Analog metal-gear servo; no position feedback; stall current high |

Use this stack to learn pulse widths, BEC wiring, and horn fit. Do not design the walking current budget around twelve stalled MG995s on one cheap BEC.

## Motion — production (recommended)

| Item | Role |
| --- | --- |
| STS3215-class TTL bus servo | Daisy-chain, encoder, current/temp; 12 units for 12 DOF + spares |
| Bus adapter / ESP32-S3 / Teensy 4.1 | Real-time loop + IMU |
| IMU (e.g. ICM-42688 / BNO085 class) | Attitude for stand/walk |

Bus servos drop the PCA9685. Confirm voltage SKU (7.4 V vs 12 V) before picking the battery pack.

## MCU comparison (short)

| Board | Keep? | Why |
| --- | --- | --- |
| Arduino Uno | Bench only | 16 MHz, 2 KB SRAM, no native high-rate USB telemetry story for 12 smart servos |
| ESP32-S3 | Recommended candidate | Cheap, USB-serial, enough CPU, Wi-Fi debug optional |
| Teensy 4.1 | Recommended candidate | Strong real-time; more cost |
| Pi 5 alone | Not for servo loop | Linux jitter; HAT pin conflict |

## Mechanical-electrical interface

Leave volume and airflow for: Pi 5 + HAT + cooler, battery, CSI ribbon, USB to MCU, and twelve servo cables (bus is fewer wires than PCA9685). Mass budget: see [design](../design/README.md).
