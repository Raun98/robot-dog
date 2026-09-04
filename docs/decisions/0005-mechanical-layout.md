# ADR-0005: v1 mechanical layout (MG995 quadruped)

- **Status:** Accepted
- **Date:** 2026-09-04

## Context

Hardware is frozen in [ADR-0004](0004-hardware-freeze.md): 12× MG995, PCA9685, ESP32-S3, Pi 5 + AI HAT+, Camera Module 3, BNO055. Mechanical docs were only qualitative. CAD must not assume ST3215 geometry, a second HAT, or a spine joint.

## Decision

v1 is a **12-DOF mammal quadruped** (coxa abduction, femur pitch, tibia pitch). No spine. Structure is 3D-printed around **MG995** bodies and **25T** horns.

Kinematic lengths, joint IDs, packaging, and print split are the numbers in [docs/design/](../design/README.md). Changing `L`, `W`, `l_coxa`, `l_femur`, or `l_tibia` by more than 5 mm, or changing the channel map, needs an ADR revision.

**CAD tool:** first sketches in **FreeCAD**. Export STEP + STL into `/design` when Phase 4 starts. License for public release stays TBD ([ADR-0003](0003-license-tbd.md)).

## Consequences

- Firmware IK uses this length set and channel map.
- Torso volume is sized for Pi 5 + AI HAT+ + active cooler with side airflow. PCA9685 is a flying board, not a Pi HAT.
- A later bus-servo swap is a new CAD, not a drop-in.

## Alternatives considered

- Insect layout (three pitch joints, no abduction): weaker side-step; rejected.
- SpotMicro / Mini Pupper CAD as production body: cite only; do not import ([license TBD](0003-license-tbd.md)).
- Long 110–120 mm femur/tibia (large SpotMicro clones): torque-poor on ~11 kg·cm analog servos; rejected for v1.
