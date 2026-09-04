# ADR-0003: License and public design

- **Status:** Proposed / TBD
- **Date:** 2026-09-04

## Context

The dog will be physically built by the owner. Whether CAD, firmware, and docs are published as open source has not been decided.

## Decision

**No LICENSE file with a chosen SPDX identifier until the owner picks a path.**

Candidates (not selected):

- Software/docs: MIT or Apache-2.0
- Hardware/CAD: CERN-OHL-S/W, or keep CAD private while software is MIT

Do not copy another quadruped's CAD into this repo. Reference projects may be cited in `docs/research/references.md`.

## Consequences

- Agents must not add a root `LICENSE` that implies a choice.
- If files are published later, this ADR is updated to Accepted with the chosen licenses.

## Alternatives considered

- Default to MIT for everything now: deferred until the owner decides.
- Proprietary / no public repo: the GitHub remote already exists; visibility can still be private without a SPDX license.
