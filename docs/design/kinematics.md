# Kinematics (v1)

Source of truth with [ADR-0005](../decisions/0005-mechanical-layout.md). Measure printed parts and update this file if reality differs by more than 2 mm.

## Frames

- **Body origin:** center of the hip rectangle (midway between the four coxa axes).
- **Axes (REP-103):** `X` forward, `Y` left, `Z` up.
- **Legs:** `FR` front-right, `FL` front-left, `RR` rear-right, `RL` rear-left. Right is `-Y`.

Each leg is three revolute joints:

| Joint | Motion | Axis (leg in standing, coxa = 0) |
| --- | --- | --- |
| Coxa | Abduction / adduction | Body `X` |
| Femur | Hip pitch | Body `Y` (after coxa) |
| Tibia | Knee pitch | Body `Y` (after femur) |

No hip yaw (about `Z`) in v1.

## Lengths (mm)

| Symbol | Value | What to measure on the print |
| --- | --- | --- |
| `L` | 175 | Front coxa axis to rear coxa axis (along `X`) |
| `W` | 80 | Left coxa axis to right coxa axis (along `Y`) |
| `l_coxa` | 40 | Coxa axis to femur pitch axis (outboard along ±`Y` at coxa = 0) |
| `l_femur` | 80 | Femur pitch axis to knee pitch axis |
| `l_tibia` | 90 | Knee pitch axis to foot contact (rubber pad) |

Hip rectangle is 175 × 80 mm. Overall stance width at coxa = 0 is about `W + 2×l_coxa` = **160 mm** plus foot thickness.

These are **short** on purpose. MG995 stall is about **10–11 kg·cm at 6 V** (~1.0–1.1 N·m). A 110 mm femur turns a 5 N static leg load into a larger hip moment and leaves no margin for walk. Do not lengthen links to look like Spot.

## Joint zeros and standing pose

Zeros are **mechanical**, not “looks level on the bench.”

| Joint | 0° | Positive direction |
| --- | --- | --- |
| Coxa | Femur pitch axis directly below the coxa axis (sagittal plane) | Positive abducts the foot **outboard** |
| Femur | Femur along `−Z` (straight down) | Positive rotates the knee **rearward** (dog-style for all four legs) |
| Tibia | Tibia collinear with femur (knee fully open) | Positive **flexes** the knee (foot moves toward the hip) |

**Standing (planning, all four legs):**

- Coxa `0°`
- Femur `+55°`
- Tibia `+95°`

That puts the foot roughly under the hip pitch axis at hip height ≈ **105–115 mm**. Firmware may trim ±10° after horn calibration. Never command tibia near `0°` on the floor (singular, servo stall).

**Sit:** femur `+80°`, tibia `+120°` (or until the belly tray is just off the floor). Record the final numbers after the first chassis print.

## Joint limits (PWM must respect these)

Mechanical stops in the printed brackets, plus conservative firmware limits:

| Joint | Mechanical (design) | Firmware v1 (tighter) |
| --- | --- | --- |
| Coxa | −35° … +35° | −25° … +25° |
| Femur | +10° … +90° | +20° … +80° |
| Tibia | +40° … +140° | +55° … +125° |

MG995 analog travel is often only ~±60° from the horn center. **Calibrate each horn so standing is near the middle of the servo’s comfortable range**, not at a rail. If a clone only does ~120° total, shrink the firmware window; do not grind the bracket.

## Channel map (PCA9685)

Matches [channel map](../hardware/channel-map.md). Do not renumber without an ADR.

| Ch | Leg | Joint |
| --- | --- | --- |
| 0 | FR | coxa |
| 1 | FR | femur |
| 2 | FR | tibia |
| 3 | FL | coxa |
| 4 | FL | femur |
| 5 | FL | tibia |
| 6 | RR | coxa |
| 7 | RR | femur |
| 8 | RR | tibia |
| 9 | RL | coxa |
| 10 | RL | femur |
| 11 | RL | tibia |
| 12–15 | — | spare (fan / unused) |

## Actuator envelope (CAD)

TowerPro-class **MG995** (owned clones may vary; caliper before first print):

- Body ≈ **40.7 × 19.7 × 42.9 mm**
- Mass ≈ **55 g** each (12 × 55 g = **660 g** of servos)
- Spline **25T**, horn retaining screw typically **M3**
- Mounting tabs: four screws; design slots, not press-fits, so clones still fit
- Lead ~300 mm; strain-relieve at the hip, do not rely on the servo grommet

CAD the **servo as a solid with tab holes**, then wrap 2–3 mm walls. Leave 0.3 mm clearance on body width.

## Torque and mass (honest budget)

Planning dry mass **1.6–2.2 kg** without a pack (see [packaging](packaging.md)). Static load per leg at 2 kg ≈ 5 N. Knee moment with a 90 mm horizontal tibia ≈ 0.45 N·m, under stall, but **walk peaks and binding will stall analog servos**. Mitigations already in architecture: short links, conservative PWM, sling first, ACS712 trip, hardware e-stop. Do not “fix” stall by lengthening the tibia.
