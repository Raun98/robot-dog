# DIY 3D-printed joint actuators (₹1,200 cap)

Study date: **2026-09-04**. This is research, not a buy list. Canonical part: **ACT-001** in [`bom/bom.csv`](../../bom/bom.csv). Cite designs; do not copy vendor CAD.

**Cap:** ≤ ₹1,200 **all-in per joint** for 12 hips/knees/tibias: motor + printed gearbox + encoder + **share of** driver electronics + fasteners/bearings/filament.

## Verdict

A **walkable quadruped hip/knee** cannot be 3D-printed at this cap.

₹1,200 is only enough for a **weak / micro** actuator (N20-class DC gearmotor + cheap encoder + brushed H-bridge share). That is an order of magnitude below owned **MG995** stall torque (~10–12 kg·cm @ 6 V) and far below **ST3215** (~19–30 kg·cm depending on SKU).

**v1:** stay on the 12 owned MG995s. Treat DIY print as a **later bench experiment** (one joint), not a 12-way replacement. There is **no India kit** in 2026 retail that is both walkable and ≤ ₹1,200/joint.

## What “walkable” costs in public DIY

| Class | Typical all-in | Torque / mass | Fits ₹1,200? |
| --- | --- | --- | --- |
| **mjbots qdd100** | ~USD 879/joint ([mjbots](https://mjbots.com/products/qdd100-beta-3)) | Mini-cheetah class QDD | No |
| **OpenTorque / SpryDrive** | ~USD 70–150 + **ODrive** (ODrive Micro **USD 89** / joint, [shop](https://shop.odriverobotics.com/products/odrive-micro)) | Large outrunner + metal or printed planetaries; OpenTorque listed ~USD 150 + expensive bearings historically ([Hackaday](https://hackaday.io/project/159404-opentorque-actuator)) | No |
| **OpenQDD** | ~USD 247 (ODrive S1 + 90 kV motor) ([Aaed Musa](https://www.aaedmusa.com/projects/openqdd)) | Printed planetary QDD | No |
| **Printed cycloidal + 5008-class BLDC** | Author target **&lt; USD 60 excluding driver**; ~454 g, **&gt;12 N·m** in one hobby build ([scferro](https://scferro.github.io/projects/03-actuator)) | Closest “print a cheetah joint” path | Motor+bearings+encoder already ~₹3k–6k; FOC driver extra |
| **Gimbal 2208/2804 + SimpleFOC** | Motor+AS5600 often **₹1,400–2,300** in India before gearbox (e.g. Evelta 2804+AS5600 **₹2,243**; DFRobot 2208 ~USD 17) | ~**0.3 kg·cm** unreduced (300 g·cm datasheet). Printed 10:1 still ~2–3 kg·cm after losses | Over cap **and** still weaker than MG995 |
| **NEMA17 + TMC2209** | Stepper ~₹400–900 + TMC2209 ~₹700–850 ([Probots TMC2209](https://probots.co.in/mks-tmc2209-v2-stepper-motor-driver-uart-module.html) ~₹719–849) | Heavy (~0.3 kg each × 12); holding torque without a gearbox is MG995-adjacent but mass/heat kill a dog | Over budget **or** overweight before print |
| **N20 + print + AS5600 + H-bridge share** | Can land **₹600–1,200** | Stall **~0.8 kg·cm** on a typical 6 V 150 RPM listing ([Robocraze N20](https://robocraze.com/products/n20-150-rpm-dc-motor-for-robotics-high-torque)) | **Yes — micro only** |

Feetech-style **metal bus servos** are the cheap *commercial* smart-servo path, not DIY print: ST3215 **₹1,799–2,649** (Rees52 / Hubtronics / ThinkRobotics), STS3215 **₹2,899** (Tomson). Still above the cap, but a finished product.

## BOM that actually fits ₹1,200 (weak)

Example **one joint**, India hobby shops, GST in list prices (spot 2026-09-04):

| Piece | ≈ ₹ | Role |
| --- | --- | --- |
| N20 metal gearmotor 6–12 V | 240–270 | Prime mover ([Robocraze N20](https://robocraze.com/products/n20-6v-60-rpm-micro-metal-gear-box-dc-motor)) |
| AS5600 module + magnet | 120–200 | Output angle ([Probots AS5600](https://staging.probots.co.in/magnetic-encoder-sensor-as5600.html) ~₹149) |
| Printed planet/cycloid + 1–2 608/625 bearings | 80–250 | Extra reduction; **adds backlash** |
| Driver **share** (TB6612 / cheap dual H-bridge; 1 board / 2 motors) | 40–120 | Not FOC |
| Fasteners, wire, PETG | 50–150 | |
| **Total** | **~₹550–1,000** | Under cap |

Do **not** count a second ESP32 per joint. One gait MCU already exists (MCU-002).

**2208/2804 “cheap BLDC”** does not fit the cap once you add a real FOC board. SimpleFOC Mini is ~€12 **plus** a motor that is already ~₹1.5k–2.3k in India. Amortizing one ODrive across 12 motors is not how FOC hardware works: you need **one power stage per motor**.

## What fails at this price (even if you “make it work” on the bench)

- **Torque:** N20 + printed stage cannot hold a Mini-Pupper-scale femur against gravity the way MG995 metal gears can. Gimbal BLDCs are camera motors (~0.07 N·m GB2208-class, T-Motor list), not leg motors.
- **Backlash:** FDM cycloidal/planetary teeth are sloppy; gait IK assumes a repeatable joint. Printed cycloids in hobby write-ups often show **~50–70%** gearbox efficiency.
- **Heat / creep:** PLA gears soften; PETG/ABS still creep under stall. MG995 already runs hot; a printed box around an N20 will strip first.
- **Print strength:** Tooth shear and horn cracks at stall — the usual hobby-servo failure, without the metal gear train.
- **Electronics:** Twelve FOC drivers do not get cheaper by sharing a UART. TMC2209 × 12 ≈ ₹8.5k **drivers only**. ACS712 on the **pack** (CUR-001) does not replace per-joint current sense.
- **Time:** CAD, PID, and 12 unique calibrations vs PWM into owned MG995s.

## Compare to what you already have / might buy

| Actuator | ≈ ₹ / joint (India) | Feedback | Dog legs? |
| --- | --- | --- | --- |
| **MG995 (owned)** | 250–450 replacement | No | **v1 yes** (open-loop, stall BEC) |
| **DIY @ ₹1,200** | 550–1,200 if N20 path | Optional AS5600 | **No** (micro) |
| **ST3215 / STS3215** | 1,800–2,900 | Encoder + load | Yes; **₹22k–35k** for 12 |
| **QDD / 5008 cycloid DIY** | 5,000+ landed | Encoder + FOC | Maybe; not this budget |

## Recommendation

1. **Walk v1 on MG995 + PCA9685.** Do not print 12 joints.
2. **Do not buy ST3215** until that walk fails (ADR-0002).
3. **Optional later:** one N20+AS5600 bench joint to learn encoders — or one 2208+SimpleFOC **demo**, knowing it is not a hip.
4. If a future ADR wants printed QDD, budget like **OpenTorque-class** (tens of thousands INR for twelve), not ₹1,200.

References for later agents: [references.md](references.md).
