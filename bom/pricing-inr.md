# INR price study

Spot check **2026-09-04** from Indian hobby shops (Robocraze, Hubtronics, Rees52, ElectroPi, Graylogix, CrazyPi, ThinkRobotics, Tomson, Probots, Evelta). GST-inclusive list prices move weekly. Use the ranges, not a shopping cart.

Canonical parts: [`bom.csv`](bom.csv). This file is **estimate only**; change `bom.csv` when you actually buy.

DIY printed joints: [`docs/research/diy-actuators.md`](../docs/research/diy-actuators.md).

## MPU-6050 — is it OK?

**Yes for v1** (stand / slow walk pitch and roll).

| | MPU-6050 (GY-521) | Step-up |
| --- | --- | --- |
| India price | **₹150–200** ([Robocraze](https://robocraze.com/products/mpu-6050-triple-axis-accelerometer-gyroscope-module) **₹154**) | BNO055 ~₹1,500; BNO085 ~₹2,400–3,000 |
| Axes | 6 (accel + gyro) | 9 with mag + on-chip fusion (BNO) |
| Yaw | Drifts (no magnetometer) | Better heading |
| Clones | Very common; still usable | Fewer fakes on branded boards |
| Role | Complementary filter on the MCU | Less filter work |

It does **not** replace joint encoders. **Frozen IMU is BNO055** (pay extra now for on-chip fusion). MPU-6050 remains an optional spare.

## ST3215 / clones (~₹2,000 each)

**Rejected** (ADR-0004). Twelve × ₹2,000 ≈ ₹24,000. Not a v1 or v1.5 buy.

| Option | ≈ ₹ / motor | 12× | Feedback | Dog legs? |
| --- | --- | --- | --- | --- |
| Feetech SC-0090 class bus | ~799–999 | ~9,600–12,000 | Yes | **No** (~2.3 kg·cm) |
| Analog 15–20 kg PWM (MG996-class) | ~350–800 | already own MG995 | **No** | Same class as now |
| Waveshare ST3215 / Feetech STS3215 | **~1,800–2,900** | **~22k–35k** | Yes | Yes, and this is the ₹20k problem |
| AliExpress “STS clone” | maybe ~1,000 landed | ~12k | Maybe | QC/warranty lottery; not a BOM SKU yet |

Shop examples: [Rees52 ST3215 ₹1,799](https://rees52.com/products/waveshare-30kg-serial-bus-servo-st3215-serial-bus-servo-with-programmable-high-torque-360-rotation-and-magnetic-encoder-servo-powerful-30kg-servo-for-demanding-robotics-applications-rs8381), [Hubtronics ST3215 ₹2,199](https://hubtronics.in/st3215-servo), [ThinkRobotics ST3215 ₹2,650](https://thinkrobotics.com/products/st3215-series-serial-bus-servo), [Tomson STS3215 ₹2,899](https://www.tomsonelectronics.com/products/serial-bus-servo-motor-sts3215-7-4v-19kg-360-degree).

Keep ST3215 `rejected`. Do not buy twelve 2 kg·cm bus micros.

## DIY 3D-printed actuator @ ₹1,200?

**Not for a walkable hip/knee.** Only an N20 + print + AS5600 + H-bridge **share** fits the cap, and stall is ~**0.8 kg·cm** vs MG995 **~10–12 kg·cm**. Gimbal 2208/2804 + FOC (Evelta 2804+AS5600 **₹2,243** before gearbox/driver) overshoots the cap and is still weak. QDD/cycloidal (OpenTorque, 5008 prints, mjbots qdd100 **USD 879**) is another decade of rupees. **Stay on MG995 for v1.**

## MCU: do not stack Teensy on a tight budget

| Board | ≈ ₹ | Verdict |
| --- | --- | --- |
| ESP32-S3 DevKit (N16R8 class) | **500–1,000** | **Frozen** gait MCU (ADR-0004). Disable Wi-Fi while walking. |
| Teensy 4.1 | **3,400–4,000** ([Robocraze](https://robocraze.com/products/teensy-4-1-development-board)) | Optional later for analog pots; skip now |
| Arduino Uno | owned | Bring-up only |

Frozen incremental electronics: **ESP32-S3 + BNO055 + ACS712 30A + level shifter ≈ ₹2,200–3,500**, plus stall-rated BEC if needed. Camera Module 3 ≈ ₹3,050–3,200 extra.

## Current sense

Stock **INA219** boards are often **±3.2 A** — too small for twelve MG995s. Prefer a **20–30 A** hall sensor (ACS712-20A/30A): [Robocraze 30A ₹79](https://robocraze.com/products/30a-acs712-current-sensor), [Hubtronics 30A ₹82](https://hubtronics.in/acs712-30a-current-sensor-module), [Probots 30A ₹235](https://probots.co.in/current-sensor-ac-dc-module-with-analog-output-acs712-30a.html). On ESP32-S3, divide the 5 V analog out. Custom-shunt INA is OK only if the shunt is stall-sized.

## Scenario totals (excl. filament, GST surprises, shipping)

**Already owned (do not rebuy):** Pi 5 8GB + AI HAT+ 26 TOPS + Uno + PCA9685 + 12× MG995. Replacement value of compute alone is ~₹20k–₹30k if lost.

| Scenario | What you still buy | ≈ ₹ |
| --- | --- | --- |
| **A. Frozen walk electronics** | ESP32-S3, BNO055, ACS712 30A, level shifter, e-stop, USB, 6 V 20–30 A BEC if needed | **2,500–5,500** |
| **A + see/hear** | A + Camera Module 3 + USB mic + cooler/SD/PSU if missing | **+3,500–6,000** |
| **C. ST3215 / clones** | 12× ~₹2k | **~₹24,000** — **rejected** |
| **D. DIY print @ ₹1.2k × 12** | 12× N20-class | **rejected** for walking |

v1 plan: **scenario A + Camera Module 3**. Not C, not D.

## Unit ranges (see also [`pricing-inr.csv`](pricing-inr.csv))

| id | Item | qty | status | ≈ ₹ / unit | ≈ ₹ extended | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| COMP-001 | Pi 5 8GB | 1 | owned | 10,000–17,000 | — | CrazyPi-class list |
| COMP-002 | AI HAT+ 26 TOPS | 1 | owned | 9,650–11,750 | — | |
| COMP-003 | Pi 5 active cooler | 1 | recommended | 400–800 | 400–800 | |
| COMP-004 | microSD 64GB A2 | 1 | recommended | 500–900 | 500–900 | |
| COMP-005 | 27W USB-C PSU | 1 | recommended | 800–1,500 | 800–1,500 | Official often higher |
| CAM-001 | Camera Module 3 | 1 | recommended | 3,050–3,200 | 3,050–3,200 | Wide costs more |
| AUD-001 | USB microphone | 1 | recommended | 200–800 | 200–800 | |
| AUD-002 | USB speaker | 1 | optional | 300–1,000 | 300–1,000 | |
| MCU-001 | Arduino Uno | 1 | owned | 400–1,200 | — | |
| MCU-002 | ESP32-S3 DevKit | 1 | recommended | 500–1,000 | 500–1,000 | Frozen gait MCU |
| MCU-003 | Teensy 4.1 | 1 | optional | 3,400–4,000 | 3,400–4,000 | Skip now |
| DRV-001 | PCA9685 | 1 | owned | 200–450 | — | |
| LVL-001 | I2C level shifter | 1 | recommended | 50–150 | 50–150 | 3.3/5 V |
| SRV-001 | MG995 | 12 | owned | 250–450 | — | |
| SRV-002 | ST3215 / clones | 12 | rejected | 1,800–2,900 | 21,600–34,800 | ~₹2k clones still ~₹24k |
| ACT-001 | DIY printed joint | 12 | rejected | 550–1,200 | 6,600–14,400 | Not walkable |
| IMU-001 | BNO055 | 1 | recommended | 1,500–2,000 | 1,500–2,000 | Frozen body IMU |
| IMU-002 | MPU-6050 GY-521 | 1 | optional | 150–200 | 150–200 | Spare |
| CUR-001 | ACS712 30 A | 1 | recommended | 80–235 | 80–235 | Not INA219 3.2 A |
| PWR-001 | 6 V 20–30 A BEC | 1 | recommended | 400–1,200 | 400–1,200 | Buy if bench PSU is weak |
| PWR-002 | LiPo 3S class | 1 | deferred | 1,500–3,000 | 1,500–3,000 | |
| PWR-003 | 5 V 5 A+ buck | 1 | deferred | 400–1,200 | 400–1,200 | |
| PWR-004 | Fuse + e-stop | 1 | recommended | 100–400 | 100–400 | |
| CBL-001 | USB Pi–MCU | 1 | recommended | 50–200 | 50–200 | |
| CBL-002 | CSI ribbon | 1 | recommended | 100–400 | 100–400 | If not in camera kit |
| MECH-001 | Print filament | 1 | deferred | 800–2,500 | 800–2,500 | PETG/ABS; design TBD |

Sources (examples): [Robocraze MPU-6050](https://robocraze.com/products/mpu-6050-triple-axis-accelerometer-gyroscope-module), [Hubtronics ST3215](https://hubtronics.in/st3215-servo), [Rees52 ST3215](https://rees52.com/products/waveshare-30kg-serial-bus-servo-st3215-serial-bus-servo-with-programmable-high-torque-360-rotation-and-magnetic-encoder-servo-powerful-30kg-servo-for-demanding-robotics-applications-rs8381), [Tomson STS3215](https://www.tomsonelectronics.com/products/serial-bus-servo-motor-sts3215-7-4v-19kg-360-degree), [Robocraze Teensy 4.1](https://robocraze.com/products/teensy-4-1-development-board), [Graylogix ESP32-S3](https://www.graylogix.in/product/node-mcu-esp32-s3-n16r8-devkit), [ElectroPi Camera 3](https://www.electropi.in/raspberry-pi-camera-module-3), [Robocraze ACS712](https://robocraze.com/products/30a-acs712-current-sensor), [Evelta 2804+AS5600](https://evelta.com/2804-3-phase-brushless-dc-motor-12v-2600rpm-300g-cm/).
