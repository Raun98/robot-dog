# PCA9685 channel map (v1)

Planning document. Implementation sketches later live in `/hardware` and `/firmware`.

Servo rail is **ATX 5 V** on the bench (6 V BEC deferred), **not** Pi or Uno 5 V. I2C to the PCA9685 is from the **ESP32-S3** through the level shifter (Phase 1 bring-up may use the Uno on the same channel numbers).

| Ch | Leg | Joint | PWM (planning) |
| --- | --- | --- | --- |
| 0 | FR | coxa | ~50 Hz analog |
| 1 | FR | femur | |
| 2 | FR | tibia | |
| 3 | FL | coxa | |
| 4 | FL | femur | |
| 5 | FL | tibia | |
| 6 | RR | coxa | |
| 7 | RR | femur | |
| 8 | RR | tibia | |
| 9 | RL | coxa | |
| 10 | RL | femur | |
| 11 | RL | tibia | |
| 12–15 | — | unused | Fan/aux later |

Zeros, signs, and length set: [docs/design/kinematics.md](../design/kinematics.md).

Pulse microseconds are **not** frozen here. Each MG995 clone differs. Phase 1 records `pulse_min`, `pulse_mid`, `pulse_max` per channel into a table in `/firmware` when that work starts.
