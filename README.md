# Open Humanoid Platform

An open, India-built robot joint standard — proven with a small bipedal
humanoid, seeded through a cheap desktop kit. **Pre-formation, pre-hardware.**
Nothing has been built yet; this repo is currently the plan and the journal.

The honest framing: this is a **sovereignty and openness play, not a cost
play**. India imports the large majority of core robotics components and
cannot currently manufacture robot actuators at any scale. The goal is not to
beat commodity Chinese actuator pricing (CubeMars alone ships ~6M units/year)
— it's to own provenance, transparency, and support for buyers who
structurally can't or won't rely on that supply chain.

## Three artefacts

| | What | Why |
|---|---|---|
| **v0 prototype** | 45cm, 22-DOF, 3D-printed bipedal humanoid | Proves the tech, builds credibility. Not the product. |
| **Actuator** | One SKU (~15 Nm, quasi-direct-drive) + a published open joint interface | The actual product and revenue line. |
| **Kit** | ~₹5,000 desktop torso, phone-as-brain, 12 DOF | Builds the community/fleet/data layer via India's Atal Tinkering Labs network. |

## Where things stand

Pre-hardware, pre-code, pre-incorporation. Nothing has been printed, wired,
or flashed yet — see the build order in `docs/v0-build-spec.md`, which
deliberately starts with a single servo on a bench, not a finished robot.
Fabricating CAD, firmware, or simulation models ahead of that physical
groundwork would just be invented numbers standing in for measurements that
don't exist yet, so this repo doesn't contain any of that scaffolding.
`cad/`, `electronics/`, and `software/` are placeholders describing what
belongs in each once the corresponding build step is real.

## Documents

- **[docs/comprehensive-report-2026-08.md](docs/comprehensive-report-2026-08.md)**
  — the plan of record: thesis, market/competitive read (CubeMars, Cosine,
  xTerra), funding map, regulatory gates (BIS/CRS/DPDP/FCC), risk register,
  open decisions.
- **[docs/18-month-plan.md](docs/18-month-plan.md)** — the same plan, spelled
  out quarter by quarter: milestones, budget line items, pre-committed gates
  (month 10 / 13 / 18), grant calendar.
- **[docs/v0-build-spec.md](docs/v0-build-spec.md)** — the 45cm prototype:
  physical dimensions, 22-DOF breakdown, actuation classes, electronics,
  software stack, "done means" checklist, build order, BOM.
- **[docs/mass-market-kit-spec.md](docs/mass-market-kit-spec.md)** — the
  ₹5,000 kit: design rules, BOM, 5-tier product ladder, electronics, the
  servo-potentiometer feedback hack, localization plan, ATL route to market.
- **[research/rd-journal.md](research/rd-journal.md)** — the highest-value
  artifact in this repo. A dated log of what was tried, what worked, what
  failed, and why — updated at minimum quarterly. See its own header for the
  entry template and the reasoning behind keeping it honest.

`CLAUDE.md` has a denser cross-referenced summary of all of the above, written
for an AI coding agent picking up this repo cold — useful as an orientation
doc for a human too.

## Milestones (the spine of the 18-month plan)

| # | Milestone | By |
|---|---|---|
| M1 | Torque loop closes on a gimbal motor | Month 2 |
| M2 | One 6-DOF leg tracks a trajectory | Month 5 |
| M3 | Robot walks 2 m unassisted | Month 9 |
| M4 | Custom joint survives 100 h under load | Month 12 |
| M5 | Someone else builds one from the repo | Month 14 |
| M6 | First actuators sold | Month 17 |

## Repo layout

```
docs/         strategy, plans, and specs (populated)
research/     the R&D journal (populated)
cad/          source CAD + exported STL/STEP (empty — starts at M1/M2)
electronics/  wiring diagrams, India-sourceable BOM with alternates (empty)
software/     phone/kit app, ESP32 + STM32G4 firmware, ROS 2 stack (empty)
```

## License

Not yet finalized or applied to any files. Current intent, per
`docs/comprehensive-report-2026-08.md`: hardware/CAD under **CERN-OHL-W**,
firmware/software under **Apache-2.0**, docs under **CC-BY-SA 4.0**. A
`LICENSE` file for software is included now since the exact text is
unambiguous; hardware and docs licenses will be added once confirmed with the
license text in hand rather than reproduced from memory.
