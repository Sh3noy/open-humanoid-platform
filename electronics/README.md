# electronics/

Wiring diagrams and India-sourceable bills of materials (with alternates) for
the v0 prototype, the custom actuator, and the mass-market kit.

**Empty.** The BOMs in `../docs/v0-build-spec.md` and
`../docs/mass-market-kit-spec.md` are cost estimates for planning, not
sourced/verified parts lists yet — no supplier has been confirmed, no wiring
diagram has been drawn against a real build. When that work starts:

- One BOM per artefact (`v0-bom.csv`, `actuator-bom.csv`, `kit-bom.csv`),
  each with at least one Indian-sourceable alternate per line item.
- Wiring diagrams for the servo bus, the custom joint's CAN-FD harness, and
  the kit's ESP32/PCA9685 harness.
- Intended license: CERN-OHL-W (see `../README.md`) — not yet applied.
