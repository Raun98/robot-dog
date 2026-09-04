# Bill of materials

This folder is the **only** parts list. Status values:

| Status | Meaning |
| --- | --- |
| `owned` | Already on the bench; keep for learning even if not used on the walking dog |
| `recommended` | Intended production (or strongly suggested) part |
| `optional` | Nice to have for v1 |
| `deferred` | Needed later; SKU not locked |

Edit [`bom.csv`](bom.csv) for every add, qty, or status change. Architecture changes also need an ADR under `docs/decisions/`.

## CSV columns

`id,category,name,qty,status,role,notes`

## Why not Uno + MG995 as the walking robot

- **Uno:** too little RAM/CPU for 12-DOF gait + IMU + telemetry; 5 V I2C vs Pi 3.3 V; AI HAT+ blocks stacking a PWM HAT on the Pi. Keep as Phase 1 jig ([ADR-0002](../docs/decisions/0002-motion-stack.md)).
- **MG995:** analog, no position/current feedback, high stall current, poor long-term walking durability. Use for PWM/power practice only.
- **PCA9685:** correct for analog PWM; unnecessary once TTL bus servos are chosen.

## Owned vs recommended (summary)

Compute is owned/fixed (Pi 5 + AI HAT+). Motion production path is bus servos + 32-bit MCU (`recommended` rows). Camera, mic, battery pack SKUs are `recommended` or `deferred` until purchase.
