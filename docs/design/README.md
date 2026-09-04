# Mechanical design

CAD and meshes later live in `/design`. License for public release is TBD ([ADR-0003](../decisions/0003-license-tbd.md)). Layout freeze: [ADR-0005](../decisions/0005-mechanical-layout.md).

| Doc | Contents |
| --- | --- |
| [kinematics.md](kinematics.md) | 12-DOF lengths, zeros, limits, PCA9685 channels |
| [packaging.md](packaging.md) | Pi 5 + AI HAT+, camera, IMU, BEC placement |
| [print-plan.md](print-plan.md) | Printed parts, PETG, fasteners, assembly order |

## Rules

- **12 DOF:** coxa / femur / tibia. No spine in v1.
- CAD around **MG995** bodies and **25T** horns. ST3215 is not a hidden assumption.
- Torso fits Pi 5 + AI HAT+ + active cooler + airflow. No second HAT on the 40-pin.
- Cite Mini Pupper, SpotMicro, Stanford Pupper for ideas. Do not import their CAD.

## CAD tool

**FreeCAD** for the first sketches ([ADR-0005](../decisions/0005-mechanical-layout.md)). Export STEP + STL into `/design` in Phase 4.

## Fabrication

Early legs: 3D-printed brackets around MG995. Metric M2 / M2.5 / M3 as in the [print plan](print-plan.md) and [BOM](../../bom/bom.csv).
