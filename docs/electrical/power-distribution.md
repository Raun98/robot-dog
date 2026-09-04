# Power distribution (v1)

Frozen by [ADR-0004](../decisions/0004-hardware-freeze.md). Implementation drawings later in `/hardware`.

## Block

```text
                    +-- fuse F1 (pack +) --+
                    |                      |
Battery or bench    |    +-- buck 5V >=27W --> Pi 5 + AI HAT+ + camera + USB MCU
(or two PSUs)    ---+    |
                    |    +-- BEC 6V 20-30A --> e-stop (cut V+) --> ACS712 --> PCA9685 V+ --> 12x MG995
                    |
                    +-- star GND (pack -/PSU -)
```

Tethered Phase 1–5 may use **two supplies** instead of one pack: official-class USB-C for the Pi, and a separate 6 V high-current supply/BEC for servos. Tie grounds at **one** PDB star point.

## Rail ratings

| Rail | Voltage | Continuous budget | Peak / stall | BOM |
| --- | --- | --- | --- | --- |
| Compute | 5 V | ≥27 W (Pi + Hailo + Wi-Fi + USB) | Short 5 A-class USB-C | PWR-003 deferred; COMP-005 tethered |
| Actuator | ~6 V | Walk average TBD (measure Phase 4) | Plan **20–30 A** for twelve MG995 stall | PWR-001 |
| Logic (PWM chip, ACS712) | 5 V | Tens of mA | Not stall | From MCU USB 5 V, **not** BEC stall bus |
| MCU / IMU | 3.3 V | ESP32 onboard regulator | — | USB from Pi |

MG995 datasheets commonly list stall on the order of **~1.5–2.5 A** at 6 V. Twelve stalled at once is a **~20–30 A** event. Size BEC, MOSFET/relay, and main +/− for that, not for idle.

## Conductors (planning)

| Run | Planning gauge | Why |
| --- | --- | --- |
| Pack/BEC to PDB to e-stop to ACS712 to PCA9685 V+ | 12 AWG (10 AWG if long run / 30 A continuous) | Stall current |
| Each MG995 lead | Stock servo wire (~22 AWG) | Short runs; harness, not the main bus |
| I2C, e-stop sense, ACS712 analog | 24–28 AWG | Signal |
| USB Pi ↔ ESP32 | `CBL-001` as sold | Data + MCU 5 V only |

F1: below wire ampacity, above expected walk current. Exact A after Phase 4 measurement; start with a **slow-blow ~25–30 A** class on a 12 AWG 6 V bus, or a resettable breaker in that range.

## PCA9685 power pins

- **V+:** 6 V servo rail **after** e-stop (and after ACS712 if the hall is in series with V+).
- **VCC:** 5 V logic from Uno (Phase 1) or ESP32-S3 USB 5 V (v1). Do not jumper VCC to V+.
- **GND:** star ground.

## What not to do

- Feed MG995 from Uno 5 V, Pi 5 V, or AI HAT+ header pins.
- Stack a PWM HAT on the Pi (ADR-0001 / 0004).
- Return 12-servo current through the USB shield/cable.
