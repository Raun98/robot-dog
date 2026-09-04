# Agent instructions

This file is the map for humans and AI working in this repository. Prefer it over chat history.

## Project state

- **Phase:** planning (Phase 0). Hardware stack is **frozen** in `docs/decisions/0004-hardware-freeze.md`.
- **Schedule:** `docs/build-phases/action-plan.md` (part-time calendar; target first fenced walk ~14 Dec 2026).
- **Compute (fixed):** Raspberry Pi 5 8 GB + official AI HAT+ (26 TOPS). ADR-0001.
- **Motion (frozen):** 12× MG995 + PCA9685 + ESP32-S3 N16R8. ST3215/clones and DIY ₹1,200 joints are **rejected**. ADR-0002 + ADR-0004.
- **Sensing (frozen buy):** official Camera Module 3; BNO055; ACS712 30A; USB mic. INR: `bom/pricing-inr.md`.
- **License:** TBD. ADR-0003.
- **BOM:** `bom/bom.csv` only. Status `rejected` means do not purchase without a new ADR.

## Read first

1. `docs/architecture/README.md`
2. `docs/decisions/`
3. `bom/README.md` and `bom/bom.csv`
4. The matching `docs/<category>/` folder for the files you are changing

## Rules

1. Distinguish `owned` / `recommended` / `optional` / `deferred` / `rejected`. Do not add ST3215, clones, or 12 DIY actuators without a new ADR.
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
