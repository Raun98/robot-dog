# v1 action plan and calendar

Working assumption: part-time owner (~8–12 hours/week), start **2026-09-04**. Hardware is frozen ([ADR-0004](../decisions/0004-hardware-freeze.md)). Exit criteria for each phase stay in [README](README.md).

This is a **schedule**, not an architecture change. Slip dates; do not slip the stack (no ST3215, no second HAT, no Pi-driven PWM).

## North-star goal (v1)

A 12-DOF MG995 quadruped that **stands and walks** under MCU control, **sees** with Camera Module 3 + Hailo, and **hears** a wake word — in a **fenced indoor area**, with a hardware e-stop on the **servo** rail (ATX 5 V now; 6 V BEC later).

Non-goals: parkour, on-device LLM gait, cloned CAD, ROS 2 as a requirement, all-day battery. Details: [overview](../overview/README.md).

## Calendar (target)

| Window | Dates | Phase | What “done” means |
| --- | --- | --- | --- |
| W0 | 4–10 Sep 2026 | 0 closeout | Order `recommended` BOM; this plan on `main`; no new ADRs unless stack changes |
| W1–W2 | 8–21 Sep | 1 Motion bench | Uno + PCA9685 + 1–2 MG995, **ATX 5 V**, e-stop; channel map in `/hardware` |
| W2–W5 | 15 Sep–12 Oct | 2 Vision | Pi OS, cooler, 27W PSU, AI HAT+, official CSI cam, Hailo detection demo, **motors off** |
| W5–W6 | 6–19 Oct | 3 Audio | USB mic; wake word flips a Pi mode flag; still no walk |
| W3–W10 | 22 Sep–16 Nov | 4 Mechanical | MG995 CAD → print one leg → four-leg stand; ESP32-S3 + BNO055 + ACS712; stand pose |
| W11–W14 | 17 Nov–14 Dec | 5 Walk | Frozen serial protocol; sling then floor; repeatable fenced walk |

**Target first fenced walk: week of 14 Dec 2026** (~14 weeks). Phase 4 is the long pole. Phases 2–3 run in parallel with Phase 1 once the camera/PSU/cooler arrive.

Battery, ROS 2, outdoor nav, and license remain **unscheduled** ([ADR-0003](../decisions/0003-license-tbd.md)).

## Goals by layer

| Goal | Owner layer | Prove it by |
| --- | --- | --- |
| Move | ESP32-S3 + PCA9685 + 12× MG995 | Stand pose on a stand; then conservative walk in a sling |
| See | Pi 5 + AI HAT+ + Camera Module 3 | Logged detections at usable FPS with servos unpowered |
| Hear | Pi CPU + USB mic | Spoken wake word changes a mode flag |
| Safe | E-stop + fuse + serial failsafe | E-stop cuts servo 5 V before any gait; MCU sits if Pi serial dies |

## This week (W0) — do these first

1. Order every `recommended` row in [`bom/bom.csv`](../../bom/bom.csv) that you do not already own: Camera Module 3, USB mic, ESP32-S3 N16R8, I2C level shifter, BNO055, ACS712 30A, fuse + e-stop, ATX 5 V distribution kit, USB cable, CSI ribbon if needed, cooler / A2 SD / 27W PSU if missing. **Do not order the Hobbywing UBEC this week.**
2. Do **not** buy ST3215/clones, DIY ₹1,200 joints, or a Teensy.
3. Stage the **ATX** as a **servo-only** brick (5 V red, fused). Never power MG995 from Uno 5 V or Pi 5 V. Never feed the Pi from ATX 5 V.
4. Leave Phase 0 docs as the source of truth; when Phase 1 starts, put the channel map in `/hardware` and sketches in `/firmware` — not in `docs/`.

## Weekly actions

### Phase 1 — motion bench (owned parts)

- Wire 1–2 MG995 to a fused **ATX 5 V** bus; PCA9685 PWM only; logic from USB.
- Document channels 0–11 (FR/FL/RR/RL coxa–femur–tibia) in `hardware/`.
- Safe PWM sweep; prove e-stop or kill switch on the servo rail.
- Keep the Uno as a jig. Do not start gait/IK on the Uno as the production path.

### Phase 2 — vision (parallel)

- 64-bit Pi OS, cooler, official PSU, seat AI HAT+, `hailo-all`, CAM1 Camera Module 3.
- `hailortcli fw-control identify` and a Hailo detection demo at usable FPS.
- No motors attached (or servo rail off).

### Phase 3 — audio

- USB mic; lightweight wake word; optional TTS later.
- Mode flag only. Do not couple voice to walk until Phase 5 failsafe is trusted.

### Phase 4 — mechanical + gait MCU

- Pick CAD tool (FreeCAD vs Fusion/Onshape) in a short ADR when the first sketch starts.
- Print MG995 brackets; single leg, then four legs on a **stand**.
- Move PWM to ESP32-S3 + level shifter + BNO055 + ACS712.
- Exit: stand pose from Pi serial; e-stop proven on all twelve.

### Phase 5 — integrated walk

- Freeze the framed serial protocol (see [firmware](../firmware/README.md)).
- Conservative PWM maps; sling/stand first, then floor in a fenced area.
- Perception may select **slow** follow only after the failsafe is trusted.

## Parallelism (do not serialize everything)

```text
W0  buy list ────────────────────────────────┐
W1–2  Phase 1 bench (Uno jig)                │
W2–5  Phase 2 Pi + Hailo (wait on camera/PSU)┤  then Phase 3
W3–10 Phase 4 CAD / print / stand ───────────┘
W11–14 Phase 5 walk (blocked on Phase 1 + 4; vision/audio optional for first walk)
```

First walk does **not** require Hailo or wake word. First *useful* dog requires Phase 2 + 3 after the walk is safe.

## Risks that slip the calendar

| Risk | Mitigation |
| --- | --- |
| Camera Module 3 / BEC lead time | Order in W0; start Phase 1 with owned servos |
| CAD iteration | Stand-test one leg before printing four |
| MG995 stall / brownout | Stall-rated 6 V BEC; never share Pi 5 V |
| Hailo/driver mismatch | Pin HailoRT to the image; motors stay off until vision is stable |
| Scope creep (bus servos, ROS 2, battery) | New ADR or stay deferred |

## How later agents should use this file

- Treat phase **exit** rows in [README](README.md) as gates, not the calendar dates.
- If a date slips, update the table in this file in the same change.
- If the hardware stack changes, write an ADR — do not “temporarily” add ST3215 or a PWM HAT.
