# Electrical

Separate the **logic/compute rail** from the **actuator rail**. Mixing them is the usual way to brown out a Pi 5 and corrupt an SD card.

## Rails

| Rail | Typical use | Notes |
| --- | --- | --- |
| Pi 5 V (5 V, high current) | Pi 5 + AI HAT+ + CSI camera + USB MCU | Official PSU class is 5 V / 5 A when tethered. On battery, use a dedicated buck rated with headroom for Hailo + Wi-Fi bursts (plan **≥ 27 W** for compute alone before servos). |
| Servo / bus | MG995 ~6 V **or** STS3215 7.4 V / 12 V SKU | Size for **stall**, not no-load. Twelve analog servos can demand tens of amps if they stall together. Bus servos still need a pack that can pulse. |
| MCU 3.3/5 V | From USB (Pi) or a small regulator from the pack | Do not power the Uno's barrel from the servo stall rail without regulation. |

## Topology (production intent)

```text
Battery pack --> fuse --> [buck 5V Pi] --> Pi 5 + AI HAT+
                 \--> [BEC or direct for bus voltage] --> servos
USB: Pi <---> motion MCU (isolated logically; share ground with care)
E-stop: cuts servo rail (hardware), MCU sees a pin and zeros commands
```

## Grounding

- Common ground between MCU and servos is required for PWM/TTL.
- Pi USB-serial usually shares ground via USB. Avoid large servo return currents through the USB cable: star-ground at the pack / power distribution board.

## Fusing and e-stop

- Fuse or breaker on the pack positive, sized below wire rating.
- **Hardware e-stop** must interrupt servo power (MOSFET or contactor), not only a software flag.
- Pi may stay up during e-stop so logs and camera still work, or shut down — pick one in a later ADR; default is **servos off, Pi on**.

## Bench (Phase 1)

- External 5–6 V supply or a UBEC **only** for 1–2 MG995s plus Uno.
- Never feed MG995 stall current from the Uno 5 V pin.
- Logic 5 V Uno vs 3.3 V Pi: if I2C is ever used from the Pi, use a level shifter. USB-serial to Uno avoids that for Phase 1.

## Battery (deferred SKU)

Hold the exact pack until servo voltage is locked (ADR follow-up). Candidates: 2S LiPo for 7.4 V bus servos; 3S with a buck for mixed 5 V + 8.4 V. Capacity is a runtime trade against mass in the torso.
