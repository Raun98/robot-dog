# Agent instructions

This file is the map for humans and AI working in this repository. Prefer it over chat history.

## Project state

- **Phase:** planning (Phase 0). Do not invent shipped CAD, firmware, or a walking gait.
- **Compute (fixed):** Raspberry Pi 5 8 GB + official AI HAT+ (26 TOPS / Hailo-8L). See `docs/decisions/0001-compute.md`.
- **Motion:** Uno + PCA9685 + MG995 is a **bench stack**, not the production walk stack. See `docs/decisions/0002-motion-stack.md`.
- **License:** open vs closed is TBD. See `docs/decisions/0003-license-tbd.md`.
- **BOM:** `bom/bom.csv` is the only parts list. Never add a part only in a random markdown file.

## Read first

1. `docs/architecture/README.md`
2. `docs/decisions/`
3. `bom/README.md` and `bom/bom.csv`
4. The matching `docs/<category>/` folder for the files you are changing

## Rules

1. Distinguish **owned prototype parts** vs **recommended production parts**. Do not treat MG995 as the walking motors without a new ADR.
2. If you change architecture, write or update an ADR under `docs/decisions/` in the same change.
3. The AI HAT+ occupies the Pi 5 HAT/PCIe stack. Do not plan a second PWM HAT on the same 40-pin header.
4. Keep Pi 5V and servo power rails separate. See `docs/electrical/`.
5. Do not dump other projects' copyrighted CAD or vendor IP. Cite reference designs; do not copy them wholesale.
6. Implementation code belongs in `hardware/`, `firmware/`, `software/`, or `design/` once Phase 0 is left — not scattered in `docs/`.

## Where to put new work

| Kind of change | Location |
| --- | --- |
| Research, constraints, how-to for later agents | `docs/<category>/` |
| New part, qty, or status | `bom/bom.csv` (+ short note in `bom/README.md` if the schema changes) |
| Architecture fork | new ADR in `docs/decisions/` |
| Pin maps, schematics (later) | `hardware/` |
| MCU sketches (later) | `firmware/` |
| Pi / Hailo apps (later) | `software/` |
| CAD / STL (later) | `design/` |
