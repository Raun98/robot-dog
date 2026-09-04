# Power distribution (v1)

Frozen by [ADR-0004](../decisions/0004-hardware-freeze.md). Implementation drawings later in `/hardware`.

## Block (tethered bench)

```text
Official 27W USB-C --> Pi 5 + AI HAT+ + camera + USB MCU   (never ATX 5V)

ATX PS_ON to GND
ATX 5V red  --> ATO 30A fuse --> e-stop (cut V+) --> ACS712 --> fat bus (PWR-006)
                                                              --> 12x MG995 reds
ATX 12V yellow --> e-stop relay coil (optional)
ATX COM / black --> star GND (servos, PCA9685 GND, MCU GND)

PCA9685 VCC = logic 5 V from Uno/ESP32 USB. Do not feed stall current into PCA9685 V+.
```

Later (deferred): pack → fuse → 6 V BEC (`PWR-001`) onto the **same** servo bus. Pi still gets a separate ≥27 W 5 V buck (`PWR-003`), never ATX 5 V.

Tie grounds at **one** PDB star point. USB still has a ground; it must not carry stall return.

## Rail ratings

| Rail | Voltage | Continuous budget | Peak / stall | BOM |
| --- | --- | --- | --- | --- |
| Compute | 5 V | ≥27 W (Pi + Hailo + Wi-Fi + USB) | Short 5 A-class USB-C | COMP-005 tethered; PWR-003 deferred |
| Actuator (bench) | ATX 5 V | Walk average TBD (measure Phase 4) | Plan **20–30 A** for twelve MG995 stall | PWR-005 owned; PWR-006 harness |
| Actuator (mobile) | ~6 V | Same stall class | Same | PWR-001 deferred |
| Logic (PWM chip, ACS712) | 5 V | Tens of mA | Not stall | From MCU USB 5 V, **not** the servo bus |
| MCU / IMU | 3.3 V | ESP32 onboard regulator | — | USB from Pi |

MG995 datasheets commonly list stall on the order of **~1.5–2.5 A** at 6 V (somewhat less at 5 V). Twelve stalled at once is still a **~20–30 A** event. Confirm the ATX sticker **+5 V ≥ ~25 A**. Size fuse, relay, and bus for stall, not idle.

## Conductors (planning)

| Run | Planning gauge | Why |
| --- | --- | --- |
| ATX 5 V red to fuse to e-stop to ACS712 to servo bus | 12–14 AWG | Stall current (`PWR-006`) |
| Each MG995 lead | Stock servo wire (~22 AWG) | Short runs into the fat bus, not through PCA9685 copper |
| I2C, e-stop sense, ACS712 analog | 24–28 AWG | Signal |
| USB Pi ↔ ESP32 | `CBL-001` as sold | Data + MCU 5 V only |

Fuse: ATO **30 A** class on ATX 5 V red, below wire ampacity. Recheck after Phase 4 current measurement.

## PCA9685 power pins

- **Servo power:** ATX 5 V (or later 6 V BEC) on a **separate** distribution bus after e-stop. Not the board’s thin V+ plane for twelve stall loads.
- **VCC:** 5 V logic from Uno (Phase 1) or ESP32-S3 USB 5 V (v1). Do not jumper VCC to the stall bus.
- **GND:** star ground.

## What not to do

- Feed MG995 from Uno 5 V, Pi 5 V / USB-C, or AI HAT+ header pins.
- Feed the Pi from ATX 5 V.
- Stack a PWM HAT on the Pi (ADR-0001 / 0004).
- Return 12-servo current through the USB shield/cable.
- Dump twelve-servo stall through PCA9685 V+.
