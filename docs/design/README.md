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

Exact mm envelope is TBD after the production servo SKU is chosen (MG995 vs STS3215 bodies differ).

## CAD tool

TBD among FreeCAD (open), Fusion/Onshape (account-based). Pick in a later ADR when the first sketch starts. Export STEP + STL into `/design` when that happens.

## Fabrication

- Early legs: 3D-printed brackets around the **production** servo, not optimized around MG995 if those will be discarded.
- Fasteners: consistent metric (M2/M3) called out in the BOM when hardware is chosen.
- Servo horns: lock to the servo vendor’s spline; document in BOM notes.

## Cable routing

- CSI ribbon: minimum bend radius, no pinch at hip yaw.
- Bus servo chain: fewer wires than 12× PWM + power; still strain-relieve at each hip.
- Service loop so a leg can be removed without unsoldering the Pi.

## Assembly stages

Align with [build phases](../build-phases/README.md): single-leg mock → chassis with Pi tray → four legs on a stand → untethered.
