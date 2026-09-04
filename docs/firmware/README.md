# Firmware

MCU firmware. Sketches later live in `/firmware`. Stack: [ADR-0004](../decisions/0004-hardware-freeze.md).

## Responsibilities (ESP32-S3)

- Gait / IK at **50–100 Hz**.
- PCA9685: 12× MG995 PWM. Channel map: [hardware/channel-map.md](../hardware/channel-map.md). IK lengths: [design/kinematics.md](../design/kinematics.md).
- **BNO055** attitude for stand / slow walk.
- **ACS712 30A** rail current; trip to sit/e-stop on stall spike.
- Pi USB-serial commands; failsafe if the link drops.
- E-stop pin; no walk on boot; conservative pulse maps.

## Phase 1 — Uno + PCA9685 (bring-up)

Channel map 0–11, pulse ranges, external BEC. No floor trot.

## v1 gait — ESP32-S3

I2C: level-shifted PCA9685 + BNO055. Wi-Fi off while walking. UART left unused (future hook only). **Do not** implement ST3215 clones.

## Protocol sketch (to be frozen in Phase 5)

Binary framed packets, little-endian, CRC16. Example command payload (not implemented yet):

- `mode` (idle / stand / walk / estop)
- `vx, vy, yaw_rate` (float32 or Q-format)
- `flags`

Do not implement this in Phase 0; this paragraph is the contract for later agents.

## Safety hooks

See [safety](../safety/README.md). No walk on boot. Conservative PWM. Hardware e-stop overrides software.
