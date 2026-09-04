# Packaging (v1 torso)

How the [BOM](../../bom/bom.csv) fits inside the body. Kinematics: [kinematics.md](kinematics.md).

## Torso envelope

Internal tray (planning):

- Length (`X`): **190 mm** (hips at 175 mm plus wall and coxa servo pockets)
- Width (`Y`): **110 mm** inside walls (hip axes at 80 mm; coxa servos sit in the corners)
- Height (`Z`): **55 mm** clear above the tray floor for Pi 5 + AI HAT+ + cooler airflow

Do not bury the compute stack. Side walls get **vent slots** aligned with the Pi 5 active cooler intake/exhaust. The AI HAT+ owns the 40-pin header; nothing else stacks there.

## Compute orientation

Place the **Pi 5 so USB-A and Ethernet face the robot rear**. On Pi 5 the MIPI (CAM/DISP) connectors sit on the power/HDMI edge, which then faces **forward** — short CSI run to the head.

```text
                    FRONT  (+X)
              [ Camera Module 3 ]
                    CSI ribbon
           FL *                    * FR
              |  CAM/HDMI/USB-C    |
              |      Pi 5          |  GPIO along +Y or -Y
              |   + AI HAT+        |  (no second HAT)
              |   + active cooler  |
              |                    |
    BNO055    |  ESP32-S3  PCA9685 |  e-stop button (reach from rear-side)
    (center)  |  shifter  ACS712   |
              |  6V BEC + fuse     |
           RL *                    * RR
              USB-C Pi PSU / USB MCU cable
                    REAR  (-X)
```

- **CAM1** only for Camera Module 3 (architecture). Service loop in the neck; minimum bend per ribbon spec; clamp the cable 20 mm from each connector.
- **USB mic** plugs into a Pi USB-A at the rear. Do not trap the dongle inside a sealed box.
- **Pi 5V:** USB-C from the official-class 27 W PSU (tethered). Servo 6 V never ties to this rail.

## Motion cluster

Keep **I2C short**. Cluster these on the tray floor, rear-center, on standoffs:

| Part | Placement |
| --- | --- |
| ESP32-S3 N16R8 | Rear tray; USB toward rear for the Pi cable |
| I2C level shifter | Between ESP32 3.3 V I2C and 5 V PCA9685 |
| PCA9685 | Same cluster; PWM headers toward the four hips; **not** on the Pi GPIO |
| BNO055 | Rigid to the **chassis**, as close as practical to the body origin, **not** on a servo or the Pi case. Mark `+X` forward on the board silkscreen in CAD notes |
| ACS712 30A | In series with the **servo positive** after the e-stop, on the fat bus — not through PCA9685 V+ |
| 6 V BEC 20–30 A | Deferred; rear tray when mobile. Bench uses ATX 5 V off-board. |
| Fuse + e-stop | ATX 5 V red (bench) or pack/BEC positive later; button reachable without putting a hand in the legs |

Arduino Uno is **bench-only** (Phase 1). It does not get a tray mount on the walking chassis.

## Head

- Camera Module 3: **25 × 24 × 11.5 mm** (standard, not Wide unless the BOM changes).
- Mount on a printed “head” with M2 screws. Optical axis: **forward, pitched down ~15°** so Hailo sees floor + person at 1–2 m, not only sky.
- No pan/tilt servo in v1 (would steal a channel and mass). Neck is a fixed print.
- Pi 5 needs a **22-pin 0.5 mm to 15-pin** camera cable (often in the Module 3 kit; otherwise CBL-002).

## Cable routing

- **Twelve** MG995 leads (signal + 6 V + GND) to PCA9685. Group as four bundles (one per hip). Strain relief clip at each coxa. Extra length coils **in the torso**, not at the knee.
- **USB Pi ↔ ESP32:** dedicated CBL-001. Star-ground so stall current does not return through this cable ([electrical](../electrical/README.md)).
- **CSI:** no pinch at femur motion; the camera does not move relative to the Pi.
- Leave a **service loop** so one leg unbolts without desoldering.

## Mass planning (dry, no pack)

| Group | ≈ g |
| --- | --- |
| 12× MG995 | 660 |
| Pi 5 + AI HAT+ + cooler | 120–160 |
| ESP32 + PCA9685 + shifter + BNO055 + ACS712 + BEC | 80–120 |
| Camera + USB mic + cables | 40–80 |
| Printed PETG structure (first estimate) | 450–750 |
| Fasteners / inserts | 40–80 |
| **Total** | **~1.4–1.9 kg** |

Keep the (later) pack **low and centered**. Hailo + Pi stay high only as much as airflow requires.

## Access and service

- Top cover: screws, not glue. Need access to SD, CSI, USB-C, e-stop.
- Bottom cover optional; tray floor is the strength member between hips.
- Rubber / TPU pads on feet (MECH-003). Hard PLA on tile will slip and shock the gears.
