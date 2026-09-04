# Build phases

Instruction set for humans and later AI sessions. Do not skip Phase 0 completeness: docs and BOM stay accurate when parts change.

## Phase 0 — Documentation (current)

- Repo map, ADRs, hardware/software/firmware/design/electrical/safety notes, consolidated BOM.
- No requirement to flash Hailo or print parts.

**Exit:** this tree exists on `main` and agents read `AGENTS.md` first.

## Phase 1 — Motion bench (owned parts)

- Arduino Uno + PCA9685 + 1–2 of the 12 MG995s, external BEC, e-stop or kill switch.
- Channel map document in `/hardware` when written.
- **Exit:** safe PWM sweep, no Uno-powered servos, numbered channels 0–11 reserved.

## Phase 2 — Vision on Pi 5

- OS, cooler, AI HAT+, `hailo-all`, CSI camera, detection demo at usable FPS.
- **Exit:** logged detections without any motors attached (or motors powered off).

## Phase 3 — Audio

- USB (or I2S) mic; wake word; optional TTS.
- **Exit:** a spoken word changes a Pi-side mode flag (still no walk).

## Phase 4 — Mechanical prototype

- CAD for **MG995** legs (v1).
- Single-leg then four-leg on a **stand**. High-current 6 V rail for twelve servos.
- **Exit:** stand pose under MCU control; Pi serial connected; e-stop proven.

## Phase 5 — Integrated walk

- Freeze serial protocol, conservative PWM, sling/stand first walks, then floor.
- Perception may select modes (follow slowly) only after failsafe is trusted.
- **Exit:** repeatable walk in a fenced area; BOM updated to what was actually built.

## Later (not scheduled)

ST3215/STS3215 only if MG995 walk is not good enough (new ADR). ROS 2, outdoor nav, battery optimization, license publication (ADR-0003).
