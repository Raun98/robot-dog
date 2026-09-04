# Mechanical design

CAD and meshes later live in `/design`. License for public release is TBD ([ADR-0003](../decisions/0003-license-tbd.md)).

## Kinematic layout (v1)

- **12 DOF:** 3 per leg (coxa / femur / tibia). No spine pitch in v1 unless a later ADR adds it.
- Joint numbering should match firmware channel/ID maps (FR, FL, RR, RL).
- Work from a **standing pose** with femur/tibia angles that keep the CoM inside the support polygon.

Cite Mini Pupper, SpotMicro, and Stanford Pupper for *ideas*. Do not import their CAD as the production body.

## Size and mass (planning budget)

The torso must fit:

- Raspberry Pi 5 + AI HAT+ + active cooler + airflow
- Battery + PDB / fuse / e-stop
- CSI camera at the “head” with ribbon strain relief
- USB run to the motion MCU
- Twelve servos in the legs (or eight + four if a simpler mock)

Keep the pack low. Hailo + Pi are heat sources; do not bury them without vents or a duct to the cooler.

Exact mm envelope: design around **MG995** bodies and horns for v1. ST3215/STS3215 is a later mechanical change only if those servos are purchased.

## CAD tool

TBD among FreeCAD (open), Fusion/Onshape (account-based). Pick in a later ADR when the first sketch starts. Export STEP + STL into `/design` when that happens.

## Fabrication

- Early legs: 3D-printed brackets around **MG995** (v1). Do not wait for bus-servo geometry.
- Fasteners: consistent metric (M2/M3) called out in the BOM when hardware is chosen.
- Servo horns: MG995 25T-class spline; document if a pack differs.

## Cable routing

- CSI ribbon: minimum bend radius, no pinch at hip yaw.
- Twelve PWM + power leads to the PCA9685; strain-relieve at each hip. A bus chain is fewer wires only after a servo upgrade.
- Service loop so a leg can be removed without unsoldering the Pi.

## Assembly stages

Align with [build phases](../build-phases/README.md): single-leg mock → chassis with Pi tray → four legs on a stand → untethered.
