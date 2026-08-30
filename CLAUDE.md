# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Open-source, decentralized robotics platform for India. The plan of record is
`docs/comprehensive-report-2026-08.md` — read it before making any scope,
sequencing, or product-priority call. Its thesis, in short: **India cannot
make robot actuators, and that gap is worth more than another humanoid.**
This is a sovereignty/openness play, not a cost play — the goal is not to
beat Chinese actuator pricing (CubeMars ships ~6M units/year), it's to own
provenance, transparency, and support for an Indian buyer who structurally
can't or won't buy Chinese.

Three artefacts, not three versions of one thing:
- **v0 prototype** — 45cm, ~3kg, 22 DOF, 3D-printed bipedal humanoid. Proves
  the tech, builds credibility. Not the product. Full build spec (physical
  dims, DOF breakdown, actuation classes, electronics, software stack, "done
  means" checklist, build order, BOM) is in `docs/v0-build-spec.md` — read it
  before touching CAD, wiring, or the control stack. Note: the build spec
  specs Raspberry Pi 5 onboard compute with an off-board RTX 6000 Ada for
  policy training, not the phone-as-brain approach described in the earlier
  design doc — the Pi 5 is the current spec of record for v0; phone-as-brain
  is reserved for the mass-market kit (see below).
- **Actuator** — one SKU (~15 Nm, quasi-direct-drive), plus a published open
  joint interface (mounting pattern, output flange, 48V, CAN-FD). This is the
  actual product and revenue line — "Arduino, not a product company." The
  interface only gets published once a working joint exists; a spec with no
  implementation is noise.
- **Kit** — ~₹5,000 desktop torso (12 DOF, phone-as-brain), distributed
  through India's Atal Tinkering Labs network. Builds the community/fleet/data
  layer, not a walking robot. Full spec (design rules, BOM, 5-tier product
  ladder, ESP32/PCA9685 electronics, the servo-potentiometer feedback hack,
  language localization plan) is in `docs/mass-market-kit-spec.md`.

**Current state: pre-hardware, pre-code, pre-incorporation.** The repo is
docs and scaffolding only — `research/rd-journal.md`, `docs/`, a top-level
`README.md`, and empty `cad/`, `electronics/`, `software/` directories (each
holding just a placeholder `README.md` describing what belongs there and why
it's empty — see Planned repo structure below). No CAD, no chassis, no
firmware, no company entity exists yet. There is nothing to build, lint, or
test. Do not invent build/run instructions — check back here (or `git log`)
before assuming tooling exists. Do not populate `cad/`, `electronics/`, or
`software/` with placeholder engineering artifacts (guessed dimensions, fake
BOM line items, scaffolded-but-untested code) — the build order in
`docs/v0-build-spec.md` starts with real, physical, measured steps (one servo
on a bench, then weighing a printed leg before ordering more servos) for a
reason; anything invented ahead of that misrepresents itself as real data.

## Sequencing and gates

The 18-month plan (summarized in the comprehensive report, spelled out
quarter-by-quarter in `docs/18-month-plan.md`) supersedes the earlier
"2-week demand test before any build" framing from the original
`/office-hours` design doc (see Full context below) — that doc's Approach C
and "The Assignment" gate are historical context for how the project's
thinking evolved, not the current plan. The current milestone sequence:

M1 torque loop closes (month 2) → M2 one leg tracks a trajectory (month 5) →
M3 walks 2m unassisted (month 9) → M4 custom joint survives 100h under load
(month 12) → M5 someone else builds one from the repo (month 14) → M6 first
actuators sold (month 17).

Pre-committed gates — honor these if asked to plan past them:
- **Month 10**, not walking → cut arms/neck, drop to 12 DOF, ship the walk.
- **Month 13**, joint can't survive 100h → pivot to buying actuators and
  becoming a robot company (a real outcome, not a failure).
- **Month 18**, no actuator sales → component thesis is wrong for now; kit/
  education becomes the fallback business.

The single largest open technical risk: real-time bipedal balance control
over a phone's camera/IMU pipeline (latency, actuator count, torque/current a
phone can't supply standalone) is **unsolved** and does not transfer from
prior art (OpenBot, Intel Labs 2020, proved phone-as-brain only for a wheeled
rover). Treat any claim that this is "already solved" with skepticism unless
a specific test result backs it.

## Knowledge-Foundation track

Independent of whether the actuator/kit business succeeds, the repo should
stand as a reproducible foundation: a stranger with a 3D printer and a dead
Android phone should be able to rebuild the v0 prototype from the published
docs alone, with no live help beyond a couple of clarifying questions. The
R&D journal (below) is the primary artifact for this — more important than
polished docs of the parts that worked.

## The R&D journal (`research/rd-journal.md`)

This is the highest-value artifact in the repo — more important than any code
or CAD that eventually lands here. Rule: **log dead ends as carefully as
wins.** Most open hardware projects only publish what worked; that's exactly
why they're hard to build on.

- Add a dated entry every time something is tried, built, tested, or decided
  — success or failure. Minimum cadence: once per quarter, even if the entry
  is just "nothing shipped this quarter, here's why." Silence in this journal
  is itself a signal to a future reader.
- Use the entry template already in the journal file (`## YYYY-MM-DD — Title`,
  then Status / What was tried / What happened / Why if it failed / Open
  questions / Next step).
- When asked to log R&D work, append to this file — don't create a second
  journal or scatter notes elsewhere.

## Repo structure

- `/README.md` — the human-facing entry point; links every doc below.
- `/LICENSE` — Apache-2.0, applies to `/software` once it has content. Full
  text included (it's unambiguous); this is the only license file that
  exists yet.
- `/docs` — build guide, FAQ, compatibility matrix, strategy reports.
  Populated: `comprehensive-report-2026-08.md`, `18-month-plan.md`,
  `v0-build-spec.md`, `mass-market-kit-spec.md`.
- `/cad` — source CAD + exported STL/STEP. Empty; placeholder `README.md`
  only. Starts at build-spec step 2 ("one leg on a test rig").
- `/electronics` — wiring diagrams, India-sourceable BOM with alternates.
  Empty; placeholder `README.md` only.
- `/software` — phone/kit app + microcontroller firmware + FOC firmware for
  the actuator (forked from moteus/ODrive) + the v0 ROS 2/sim stack. Empty;
  placeholder `README.md` only.
- `/research` — the R&D journal.

Planned licensing (per the comprehensive report, still unconfirmed beyond the
software `LICENSE` file already in place): hardware under **CERN-OHL-W** (not
the earlier-considered OHL-S), docs under CC-BY-SA 4.0, trademark on the name
and certification mark. Their license text hasn't been added yet — reproduce
it from the actual license source when that happens, not from memory.

Entity structure: Private Limited company (not OPC — can't take equity; not a
dual Pvt Ltd + Section 8 structure either — over-engineered for a solo
founder). Not yet incorporated.

## Full context

Two documents outside/inside this repo carry the reasoning that doesn't fit
here — read the relevant one before making a call this file doesn't cover:

- `docs/comprehensive-report-2026-08.md` (in-repo) — the current plan of
  record: thesis, funding map, regulatory gates (BIS/CRS/DPDP/FCC Covered
  List), risk register, and the open decisions not yet made (name, first SKU
  torque rating, beachhead customer, full-time-vs-part-time).
- `~/.gstack/projects/root/root-unknown-design-20260830-151046.md` (outside
  repo) — the original `/office-hours` founder-diagnostic that preceded the
  comprehensive report. Useful for the "why" behind the project's earliest
  framing, but its specific plan (Approach C, the 2-week demand-test gate) has
  been superseded by the sequencing above.
