# Electrical

Separate the **logic/compute rail** from the **actuator rail**. Mixing them is the usual way to brown out a Pi 5 and corrupt an SD card.

## Rails

| Rail | Typical use | Notes |
| --- | --- | --- |
| Pi 5 V (5 V, high current) | Pi 5 + AI HAT+ + CSI camera + USB MCU | Official PSU class is 5 V / 5 A when tethered. On battery, use a dedicated buck rated with headroom for Hailo + Wi-Fi bursts (plan **≥ 27 W** for compute alone before servos). |
| Servo | MG995 5–6 V, twelve on one rail | **Bench:** owned ATX **5 V (red)** if **+5V ≥ ~25 A**. **Mobile:** 6 V BEC (deferred). E-stop cuts this rail. |
| MCU 3.3/5 V | From USB (Pi) or a small regulator from the pack | Do not power the Uno's barrel from the servo stall rail without regulation. |

## Topology (v1)

```text
Official 27W USB-C --> Pi 5 + AI HAT+   (never ATX 5V)
ATX 5V red --> fuse --> e-stop --> 12x MG995 power bus
              (PCA9685 = PWM only; do not put stall current through V+)
Later: pack --> fuse --> [6V BEC] --> same servo bus
USB: Pi <---> motion MCU (isolated logically; share ground with care)
E-stop: cuts servo rail (hardware); relay coil from ATX 12V yellow
ACS712 30A: on servo feed; divider if MCU is 3.3 V
I2C: ESP32-S3 --> level shifter --> PCA9685 + BNO055
```

## Grounding

- Common ground between MCU and servos is required for PWM/TTL.
- Pi USB-serial usually shares ground via USB. Avoid large servo return currents through the USB cable: star-ground at the pack / power distribution board.

## Fusing and e-stop

- Fuse on ATX **5 V red** (ATO **30 A** class) before the servo bus; later the same idea on pack +.
- **Hardware e-stop** must interrupt servo power (MOSFET or contactor), not only a software flag. With ATX, coil the 12 V relay from **yellow 12 V**.
- Pi may stay up during e-stop so logs and camera still work, or shut down — pick one in a later ADR; default is **servos off, Pi on**.

## Bench (Phase 1)

- Owned **ATX 5 V** for the MG995 rail (Phase 1: 1–2 servos; Phase 4–5: all twelve if **+5V ≥ ~25 A**). UBEC + 3S is deferred until untethered / 6 V.
- Never feed MG995 stall current from the Uno 5 V pin.
- Logic 5 V Uno vs 3.3 V Pi: if I2C is ever used from the Pi, use a level shifter. USB-serial to Uno avoids that for Phase 1.

## Battery (deferred SKU)

Hold the exact pack until the v1 6 V rail is sized. A 2S/3S pack plus a beefy 6 V BEC is the MG995 path. Do not buy a 7.4 V / 12 V bus-servo pack until ST3215 is actually chosen.
