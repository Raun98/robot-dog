# Bill of materials

This folder is the **only** parts list. Status values:

| Status | Meaning |
| --- | --- |
| `owned` | Already on the bench |
| `recommended` | Buy or use for v1 (not already owned) |
| `optional` | Nice to have for v1 |
| `deferred` | Later; do not buy until an ADR says so |

Edit [`bom.csv`](bom.csv) for every add, qty, or status change. Architecture changes also need an ADR under `docs/decisions/`.

## CSV columns

`id,category,name,qty,status,role,notes`

## v1 motion vs deferred bus servos

- **MG995 × 12:** owned; **v1 walking actuators**. Analog, no feedback, high stall current — walk anyway, with a stall-rated BEC and e-stop ([ADR-0002](../docs/decisions/0002-motion-stack.md)).
- **PCA9685:** owned; v1 PWM driver. Keep it until a bus-servo ADR drops it.
- **Uno:** owned; Phase 1 jig. Prefer ESP32-S3 (or similar) for gait, still on PCA9685.
- **ST3215 / STS3215:** same product family (Waveshare vs Feetech). **Deferred.** About USD 17–22 each (~USD 200–260+ for twelve). Buy only if MG995 walking is not good enough.

## Owned vs recommended (summary)

Compute is owned/fixed (Pi 5 + AI HAT+). v1 legs are the 12 MG995s. Camera, mic, high-current BEC, and a 32-bit MCU are `recommended`. Bus servos stay `deferred`.
