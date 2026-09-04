# E-stop and servo-rail current sense

Safety policy: [safety](../safety/README.md). Default: **servos off, Pi on**.

## Hardware e-stop (PWR-004)

Must interrupt **actuator V+**. A GPIO flag in firmware is not the e-stop.

```text
6V BEC + --> [fuse] --> [HIGH-SIDE SWITCH] --> ACS712 IP+ ... IP- --> PCA9685 V+ / servo reds
                              ^
                         E-stop loop
                    (NC button or latch)
```

Keep **grounds connected** so PCA9685 PWM and MG995 signal grounds stay valid. Cutting low-side return would float servo GND relative to the MCU.

### Phase 1

A **series kill switch** or automotive-style switch in the 6 V positive is enough (1–2 servos).

### Phase 4–5

Use a **40 A-class** high-side device (logic-level MOSFET with proper high-side drive, or an automotive relay/contactor). Drive/coil from a separate small 5 V/6 V control, not from ESP32 GPIO current. MCU **senses** e-stop (open/closed) on a GPIO with a pull-up; it does not “software override” a released button.

Latching mushroom + NC contact in the high-side gate/coil circuit is preferred so the rail stays off until a human reset.

## ACS712 30A (CUR-001)

Place the hall **in series with servo V+**, after the e-stop, so an open e-stop reads ~0 A and a stall still shows on the live rail.

Typical 30 A module (5 V VCC): **66 mV/A**, **VCC/2** at 0 A.

| Current | VIOUT (5 V module, ideal) |
| --- | --- |
| 0 A | 2.50 V |
| +15 A | 3.49 V |
| +30 A | 4.48 V |

ESP32 ADC is **3.3 V max**. Do **not** wire VIOUT straight to an ADC pin.

**Divider (planning):** scale ~4.5 V → ≤3.1 V, e.g. **10 kΩ series from VIOUT, 22 kΩ to GND** (ratio ≈ 0.69 → ~3.1 V at 30 A). Recalibrate on the actual module; ACS712 offset drifts with temperature.

- ACS712 VCC = 5 V logic (same as PCA9685 VCC domain).
- Module GND = star GND.
- Optional RC on the divided node (e.g. 100 nF) to tame PWM hash; firmware still needs a stall threshold, not a pretty waveform.

INA219 (3.2 A) is **rejected** for this rail.

## MCU analog

ESP32-S3 ADC is not a lab instrument. Treat ACS712 as **stall / overcurrent trip**, not coulomb counting. Conservative trip → sit or hardware e-stop per firmware docs. MG995 has no per-joint current.
