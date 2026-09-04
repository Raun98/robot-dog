# Safety

A 12-DOF metal-gear dog can pinch fingers, dump a LiPo, and walk into furniture. Treat Phase 1 bench work as live machinery.

## Mechanical

- Keep hands clear of joints when servos are powered.
- Test new gaits on a **stand / sling** so the body cannot face-plant or run.
- Horn screws and backlash: a stripped MG995 can jump; bus servos can still slam into limits.

## Electrical

- Separate Pi and servo rails ([electrical](../electrical/README.md)).
- Hardware e-stop on the **servo** rail.
- LiPo: storage charge, fire-safe bag, no puncturing packs in the chassis without protection.
- Fuse the pack. Size wires for stall, not average current.

## Firmware / software

- **Never command a walk gait without a conservative PWM range** (MG995 has no current telemetry) and a hardware e-stop. Bus-servo current limits apply only if that upgrade happens.
- Watchdog: if Pi serial dies, MCU must sit/damp, not continue the last walk command.
- Disable torque on overtemp / overcurrent when the protocol supports it.
- Do not auto-start walk on boot.

## Perception

- Vision false positives must not map to “full speed toward person” in v1. Start with stand / look / slow walk in a fenced area.

## Human procedure (Phase 1+)

1. Power MCU/servos last, Pi first (or documented order once a PDB exists).
2. Confirm e-stop works before any gait.
3. One person on e-stop during first walks.
