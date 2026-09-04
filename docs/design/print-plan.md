# Printed parts and fasteners (v1)

CAD files later go in `/design`. This is the part split so the first FreeCAD file is not one mega-body.

Cite Mini Pupper / SpotMicro / Stanford Pupper for **ideas only**. Do not import their meshes.

## Print list

Quantities assume one dog. Mirror left/right in CAD (`FL`/`RL` vs `FR`/`RR`).

| ID | Qty | Part | Notes |
| --- | --- | --- | --- |
| P-BODY | 1 | Torso tray | Hip pockets, Pi standoffs (M2.5 Pi holes), vent walls |
| P-LID | 1 | Top lid | Vents; hole for e-stop; CSI clearance |
| P-HEAD | 1 | Camera mount | M2 for Module 3; 15° down; ribbon clamp |
| P-COXA | 4 | Coxa bracket | Holds coxa MG995; output to femur servo |
| P-FEMUR | 4 | Femur | Holds femur MG995; spans `l_femur` = 80 mm axis-to-axis |
| P-TIBIA | 4 | Tibia | Holds tibia MG995; spans `l_tibia` = 90 mm to foot |
| P-FOOT | 4 | Foot | Flat + TPU pad or printed TPU |
| P-HORN | 12 | Horn adapters | 25T MG995 spline to next link; or use kit horns + printed clamp |
| P-CLIP | 8 | Cable clips | Hip + tray |

**Material:** PETG for load parts (P-COXA, P-FEMUR, P-TIBIA, P-BODY). PLA+ acceptable for P-LID, P-HEAD, P-CLIP. **Not** brittle PLA on femurs.

**Infill:** 40–60% gyroid on femurs/coxas; 3–4 perimeters. Orient so layer lines are **not** across the bending span of the femur.

## Fasteners (BOM)

| Use | Hardware |
| --- | --- |
| MG995 tabs | M3×8 or M3×10 (four per servo → 48) plus nuts or heat-set inserts in plastic |
| Horn to link | Stock MG995 M3 horn screw + threadlocker (low strength) |
| Pi 5 | M2.5 standoffs, 4×, height clears the active cooler + HAT per official stack |
| Camera / BNO055 | M2 |
| Tray / lid | M3×12 into heat-set inserts |

Do not tap PETG and call it done; **heat-set inserts** on anything that comes apart more than once.

## Assembly order (Phase 4)

1. Print one **FR** coxa+femur+tibia; caliper `l_femur` / `l_tibia`; fix CAD.
2. Bench PWM that leg on the Uno jig (Phase 1 hardware), body in a vise, **not** on the floor.
3. Print remaining three legs; bolt to tray **without** Pi.
4. Mount Pi + HAT + camera with motors **unpowered**.
5. Four-leg **stand / sling**; then ESP32 stand pose; then e-stop proof.

## What not to print

- Gearboxes or DIY QDD (ACT-001 rejected).
- ST3215 horns or 25T-to-bus adapters “just in case.”
- A PWM HAT carrier that sits on the Pi 40-pin.
