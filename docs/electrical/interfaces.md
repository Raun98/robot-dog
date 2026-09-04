# Signal interfaces (electrical)

Pin numbers wait for `/hardware` maps. This is voltage and bus policy.

## Pi 5 ↔ motion MCU

- **Physical:** USB (`CBL-001`). ESP32-S3 native USB (v1). Uno USB in Phase 1.
- **Power on that cable:** MCU board only. Not servo return.
- **Ground:** USB GND exists; **also** a dedicated PDB ground so servo current has a fat path that is not the USB cable.

AI HAT+ occupies the HAT/PCIe stack. No PWM HAT. Do not plan I2C from the Pi to PCA9685 as the production servo path.

## I2C (v1 gait MCU)

```text
ESP32-S3 3.3 V I2C  -->  bidirectional level shifter (LVL-001)  -->  5 V PCA9685
                                                      \-->  BNO055 (see below)
```

| Device | Logic | Notes |
| --- | --- | --- |
| PCA9685 breakout | Usually 5 V VCC | Requires shifter from ESP32 |
| BNO055 module | Often 3.3 V (some boards have a regulator) | Prefer 3.3 V side of the shifter if the module is 3.3 V-only; do not feed 5 V into a 3.3 V BNO |
| Phase 1 Uno | 5 V | Shifter optional if only Uno + PCA9685 |

Pull-ups: one set of pull-ups per voltage domain (3.3 V on ESP32 side, 5 V on PCA9685 side) unless the shifter board already provides them. Do not stack three sets of 2.2 kΩ.

Addresses: leave PCA9685 and BNO055 at stock unless a conflict appears; document in the hardware pin map.

Spare hook (ADR-0004): later ADS1115 on this bus for a few pots — still 3.3/5 V aware.

## PWM

PCA9685 outputs to MG995 signal wires. MG995 signal is 5 V tolerant PWM. ESP32 must **not** bit-bang twelve servos as the v1 architecture.

Channels **0–11** legs; **12–15** unused or fan/aux (no second actuator type).

## E-stop sense

Dry contact or switch to GND with MCU pull-up (3.3 V). Debounce in firmware. Open contact = **safe (no PWM / rail already cut)**.

## Camera / audio (compute rail)

- Camera Module 3: CSI power from Pi. No servo-rail voltage on that ribbon.
- USB mic/speaker: Pi USB. Budget inside the 27 W compute rail.

## Level and ESD (planning)

- TVS on the servo bus at the PDB is optional Phase 4; fuse is required.
- Do not hot-plug servo V+ with PWM already commanding a hard position; power servos last ([safety](../safety/README.md)).
