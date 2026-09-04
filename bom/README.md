# Bill of materials

This folder is the **only** parts list. Status values:

| Status | Meaning |
| --- | --- |
| `owned` | Already on the bench |
| `recommended` | Buy or use for v1 (not already owned) |
| `optional` | Nice to have for v1 |
| `deferred` | Later; do not buy until an ADR says so |

Edit [`bom.csv`](bom.csv) for every add, qty, or status change. Architecture changes also need an ADR under `docs/decisions/`.

India street prices (estimate only): [`pricing-inr.md`](pricing-inr.md) and [`pricing-inr.csv`](pricing-inr.csv).

## CSV columns

`id,category,name,qty,status,role,notes`

## v1 motion vs deferred

- **MG995 × 12:** owned; **v1 walking actuators**. Analog, no feedback, high stall current — walk anyway, with a stall-rated BEC and e-stop ([ADR-0002](../docs/decisions/0002-motion-stack.md)).
- **PCA9685:** owned; v1 PWM driver. Keep it until a bus-servo ADR drops it.
- **Uno:** owned; Phase 1 jig. **ESP32-S3** (MCU-002) is the recommended gait MCU on the same PCA9685. **Teensy 4.1** (MCU-003) is optional / skip on budget.
- **MPU-6050 GY-521** (IMU-001): recommended v1 body IMU. Not a joint encoder.
- **ACS712 20/30 A** (CUR-001): recommended on the servo BEC. Do not use a stock INA219 (±3.2 A).
- **ST3215 / STS3215:** same product family (Waveshare vs Feetech). **Deferred.** India ~₹1,800–2,900 each (~₹22k–35k for twelve).
- **ACT-001 DIY printed actuator:** **deferred study** only. A walkable hip/knee does **not** fit ₹1,200 all-in; see [`docs/research/diy-actuators.md`](../docs/research/diy-actuators.md).

## Owned vs recommended (summary)

Compute is owned/fixed (Pi 5 + AI HAT+). v1 legs are the 12 MG995s. Cheapest remaining buy path: ESP32-S3 + MPU-6050 + ACS712 + e-stop/cables (scenario A in pricing-inr). Camera/mic when ready. Bus servos and DIY QDD stay `deferred`.
