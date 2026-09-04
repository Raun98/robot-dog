# Bill of materials

This folder is the **only** parts list. Frozen stack: [ADR-0004](../docs/decisions/0004-hardware-freeze.md).

| Status | Meaning |
| --- | --- |
| `owned` | Already on the bench |
| `recommended` | Buy for v1 |
| `optional` | Nice to have; not required to freeze |
| `deferred` | Later (battery / body); not a stack change |
| `rejected` | Do not buy unless a **new ADR** |

Edit [`bom.csv`](bom.csv) for every add, qty, or status change.

India street prices: [`pricing-inr.md`](pricing-inr.md) and [`pricing-inr.csv`](pricing-inr.csv).

## CSV columns

`id,category,name,qty,status,role,notes`

## Frozen vs rejected

- **Walk:** 12× MG995 + PCA9685 + ESP32-S3 + BNO055 + ACS712 30A + e-stop. **Tethered servo power:** owned ATX 5 V. **6 V BEC + pack:** deferred.
- **See/hear:** official Camera Module 3 + USB mic. Cooler + 27W PSU if missing.
- **Rejected:** ST3215/STS3215 **and clones** (~₹2k each). DIY printed joints at ₹1,200. Teensy not required.
- **Pay extra now (upgradeability):** Camera Module 3 (Hailo CSI), BNO055 (body fusion without joint encoders), official-class Pi PSU/cooler, ESP32-S3 with native USB (not Uno forever). Skip Hobbywing UBEC until mobile.
