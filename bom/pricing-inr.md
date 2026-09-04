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

It does **not** replace ST3215 joint encoders. It tells you how the **body** is tilting. Pair with conservative gait. Skip Teensy+BNO085 if the goal is to stay off the ₹20k servo upgrade.

## ST3215 at ~₹1,000/motor?

**Not on Indian retail for MG995-class torque with a bus + encoder.**

| Option | ≈ ₹ / motor | 12× | Feedback | Dog legs? |
| --- | --- | --- | --- | --- |
| Feetech SC-0090 class bus | ~799–999 | ~9,600–12,000 | Yes | **No** (~2.3 kg·cm) |
| Analog 15–20 kg PWM (MG996-class) | ~350–800 | already own MG995 | **No** | Same class as now |
| Waveshare ST3215 / Feetech STS3215 | **~1,800–2,900** | **~22k–35k** | Yes | Yes, and this is the ₹20k problem |
| AliExpress “STS clone” | maybe ~1,000 landed | ~12k | Maybe | QC/warranty lottery; not a BOM SKU yet |

Shop examples: [Rees52 ST3215 ₹1,799](https://rees52.com/products/waveshare-30kg-serial-bus-servo-st3215-serial-bus-servo-with-programmable-high-torque-360-rotation-and-magnetic-encoder-servo-powerful-30kg-servo-for-demanding-robotics-applications-rs8381), [Hubtronics ST3215 ₹2,199](https://hubtronics.in/st3215-servo), [ThinkRobotics ST3215 ₹2,650](https://thinkrobotics.com/products/st3215-series-serial-bus-servo), [Tomson STS3215 ₹2,899](https://www.tomsonelectronics.com/products/serial-bus-servo-motor-sts3215-7-4v-19kg-360-degree).

₹1,000 × 12 = ₹12,000 would be a fair *study* budget, but **no in-stock India part** matches “bus + encoder + ~10–20 kg·cm” at that price. Keep ST3215 `deferred`. Do not buy twelve 2 kg·cm bus micros.

## DIY 3D-printed actuator @ ₹1,200?

**Not for a walkable hip/knee.** Only an N20 + print + AS5600 + H-bridge **share** fits the cap, and stall is ~**0.8 kg·cm** vs MG995 **~10–12 kg·cm**. Gimbal 2208/2804 + FOC (Evelta 2804+AS5600 **₹2,243** before gearbox/driver) overshoots the cap and is still weak. QDD/cycloidal (OpenTorque, 5008 prints, mjbots qdd100 **USD 879**) is another decade of rupees. **Stay on MG995 for v1.**

## MCU: do not stack Teensy on a tight budget

| Board | ≈ ₹ | Verdict |
| --- | --- | --- |
| ESP32-S3 DevKit (N16R8 class) | **500–1,000** | **Buy this** for v1 gait + MPU-6050 + I2C PCA9685. Disable Wi-Fi while walking. |
| Teensy 4.1 | **3,400–4,000** ([Robocraze](https://robocraze.com/products/teensy-4-1-development-board)) | Better ADCs if you later add joint pots. Skip until MG995 walk needs that. |
| Arduino Uno | owned | Bring-up only |

IMU + MCU incremental: **MPU-6050 + ESP32-S3 ≈ ₹650–1,200**, not Teensy + BNO085 (₹6k+).

## Current sense

Stock **INA219** boards are often **±3.2 A** — too small for twelve MG995s. Prefer a **20–30 A** hall sensor (ACS712-20A/30A): [Robocraze 30A ₹79](https://robocraze.com/products/30a-acs712-current-sensor), [Hubtronics 30A ₹82](https://hubtronics.in/acs712-30a-current-sensor-module), [Probots 30A ₹235](https://probots.co.in/current-sensor-ac-dc-module-with-analog-output-acs712-30a.html). On ESP32-S3, divide the 5 V analog out. Custom-shunt INA is OK only if the shunt is stall-sized.

## Scenario totals (excl. filament, GST surprises, shipping)

**Already owned (do not rebuy):** Pi 5 8GB + AI HAT+ 26 TOPS + Uno + PCA9685 + 12× MG995. Replacement value of compute alone is ~₹20k–₹30k if lost.

| Scenario | What you still buy | ≈ ₹ |
| --- | --- | --- |
| **A. Walk on a budget** | ESP32-S3, MPU-6050, ACS712 20/30 A, e-stop, USB cable, fat 6 V BEC if the bench PSU cannot do 12 servos | **1,500–4,000** |
| **A + see/hear** | A + Camera Module 3 + USB mic + cooler/SD/PSU if missing | **+3,500–6,000** |
| **B. Fancy MCU/IMU** | Teensy 4.1 + BNO055/085 | **+3,500–6,500 vs A** (avoid) |
| **C. ST3215 legs** | 12× bus servos | **+22,000–35,000** (user ~₹20k) |
| **D. DIY print @ ₹1.2k × 12** | 12× N20-class joints | **+6,600–14,400** and **still not walkable** |
| **E. DIY QDD / 5008 cycloid × 12** | Real printed actuators + FOC | **₹60k+**; not this project phase |

v1 plan: **scenario A**, then camera when ready. Not B, not C, not D, not E.

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
| MCU-002 | ESP32-S3 DevKit | 1 | recommended | 500–1,000 | 500–1,000 | Budget gait MCU |
| MCU-003 | Teensy 4.1 | 1 | optional | 3,400–4,000 | 3,400–4,000 | Do not buy for v1 |
| DRV-001 | PCA9685 | 1 | owned | 200–450 | — | |
| SRV-001 | MG995 | 12 | owned | 250–450 | — | |
| SRV-002 | ST3215 / STS3215 | 12 | deferred | 1,800–2,900 | 21,600–34,800 | Matches ~₹20k story |
| ACT-001 | DIY printed joint (N20-class) | 12 | deferred | 550–1,200 | 6,600–14,400 | Fits cap; **not walkable** |
| IMU-001 | MPU-6050 GY-521 | 1 | recommended | 150–200 | 150–200 | v1 IMU |
| IMU-002 | BNO055 | 1 | optional | 1,500–2,000 | 1,500–2,000 | If MPU is too noisy |
| CUR-001 | ACS712 20/30 A | 1 | recommended | 80–200 | 80–200 | Not INA219 3.2 A |
| PWR-001 | 6 V high-current BEC | 1 | owned/verify | 400–1,200 | 400–1,200 | Size for stall |
| PWR-002 | LiPo 3S class | 1 | deferred | 1,500–3,000 | 1,500–3,000 | |
| PWR-003 | 5 V 5 A+ buck | 1 | deferred | 400–1,200 | 400–1,200 | |
| PWR-004 | Fuse + e-stop | 1 | recommended | 100–400 | 100–400 | |
| CBL-001 | USB Pi–MCU | 1 | recommended | 50–200 | 50–200 | |
| CBL-002 | CSI ribbon | 1 | recommended | 100–400 | 100–400 | If not in camera kit |
| MECH-001 | Print filament | 1 | deferred | 800–2,500 | 800–2,500 | PETG/ABS; design TBD |

Sources (examples): [Robocraze MPU-6050](https://robocraze.com/products/mpu-6050-triple-axis-accelerometer-gyroscope-module), [Hubtronics ST3215](https://hubtronics.in/st3215-servo), [Rees52 ST3215](https://rees52.com/products/waveshare-30kg-serial-bus-servo-st3215-serial-bus-servo-with-programmable-high-torque-360-rotation-and-magnetic-encoder-servo-powerful-30kg-servo-for-demanding-robotics-applications-rs8381), [Tomson STS3215](https://www.tomsonelectronics.com/products/serial-bus-servo-motor-sts3215-7-4v-19kg-360-degree), [Robocraze Teensy 4.1](https://robocraze.com/products/teensy-4-1-development-board), [Graylogix ESP32-S3](https://www.graylogix.in/product/node-mcu-esp32-s3-n16r8-devkit), [ElectroPi Camera 3](https://www.electropi.in/raspberry-pi-camera-module-3), [Robocraze ACS712](https://robocraze.com/products/30a-acs712-current-sensor), [Evelta 2804+AS5600](https://evelta.com/2804-3-phase-brushless-dc-motor-12v-2600rpm-300g-cm/).
