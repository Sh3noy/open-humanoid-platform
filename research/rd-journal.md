# R&D Journal — Open-Source Humanoid Robot Platform

This journal exists for future engineers, not for us. Most open hardware
projects only publish what worked — that's exactly why they're hard to build
on. The rule for this journal: **log dead ends as carefully as wins.** If
something was tried and failed, say what was tried, why it failed, and what
you'd try next. A future builder inheriting this project should be able to
skip our mistakes instead of repeating them.

See the design doc for full context: `~/.gstack/projects/root/root-unknown-design-20260830-151046.md`.

## How to use this journal

Add a dated entry every time something is tried, built, tested, or decided —
whether it worked or not. Minimum cadence: once per quarter, even if the
entry is just "nothing shipped this quarter, here's why." Silence in this
journal is itself a signal to a future reader.

Entry template:

```
## YYYY-MM-DD — Title

**Status:** [in-progress | success | failed | abandoned]

**What was tried:**

**What happened:**

**Why (if it failed):**

**Open questions this raises:**

**Next step:**
```

---

## 2026-08-30 — Project genesis and initial scoping

**Status:** in-progress (pre-hardware)

**What was tried:**
Ran the idea through a structured founder-diagnostic session (`/office-hours`)
before writing any code or CAD. No prototype exists yet.

**What happened:**
The diagnostic surfaced that this idea started top-down, from a thesis (India's
e-waste volume + falling AI/compute cost + cheap sensors), not from a specific
person's observed problem. No named user, no demand evidence, no prior
personal pain point exists yet.

Landscape research found the closest prior art: **OpenBot** (Intel Labs, 2020)
— an open-source, 3D-printed robot body driven by an Android phone as the
brain. It proved phone-as-sensing-and-compute works, but only for a **wheeled**
rover with simple motor control. It was released as a research publication,
not a startup attempt, and never became a company or an ongoing community —
which is a different signal than "the market rejected it." Nobody was
actually trying to build a business or community around it.

**Why (if it failed):** n/a — this is a scoping entry, not a build attempt.

**Open questions this raises:**
- Bipedal balance control over a phone's camera/IMU pipeline (latency,
  actuator count, torque/current the phone itself can't supply) is **unsolved**
  and does not transfer from OpenBot's wheeled-motor software. This is the
  single largest open technical risk carried forward. First real build attempt
  should target getting *any* data on whether a phone-driven control loop can
  hold stable bipedal stance at usable latency — that finding either confirms
  or kills the core technical premise.
- No named target user exists. "Indian makers/students" is a category, not a
  person.
- What exactly makes humanoid (vs. wheeled, vs. static torso) the right form
  factor for this thesis, beyond "sounds cool" — not yet answered.

**Next step:**
Two tracks, running in parallel, neither blocking the other:
1. **Demand test (2 weeks):** tape an old Android phone to a cheap
   ($15-30) off-the-shelf toy humanoid skeleton, build the crudest possible
   demo (phone camera does person-detection, one servo waves an arm), and
   directly message 15 named India robotics hobbyists/teachers plus post to
   3-5 maker communities. Threshold: 3+ unprompted "where do I get this"
   requests = the platform/community track proceeds. Fewer = revisit the plan
   before spending on chassis or control R&D.
2. **This journal:** the crude demo built for the demand test *is* the first
   real R&D entry — log what the phone can and can't do (frame rate, latency,
   battery drain, which sensors are usable) the moment it exists, regardless
   of how the demand test turns out.

---

## 2026-08-30 — Plan revision: actuator-first, and repo scaffolding

**Status:** in-progress (pre-hardware)

**What was tried:**
A more detailed strategic pass (recorded as `docs/comprehensive-report-2026-08.md`)
replaced the original top-down thesis with a narrower, better-evidenced one,
plus a quarter-by-quarter execution plan (`docs/18-month-plan.md`) and full
build specs for the two hardware artefacts (`docs/v0-build-spec.md`,
`docs/mass-market-kit-spec.md`). The repo was then scaffolded to match:
top-level `README.md`, a software `LICENSE` (Apache-2.0), and placeholder
`README.md` files in `cad/`, `electronics/`, `software/` describing what
belongs in each and why they're still empty.

**What happened:**
The thesis changed materially. The original framing (2026-08-30, morning
entry above) was "humanoid platform for India's e-waste/education angle,"
gated by a 2-week demand test before any build spend. The revised framing is
narrower and more defensible: **the actuator/joint standard is the product**,
not the humanoid. Cross-checking prior art found CubeMars already ships ~6M
actuator units/year and IPO'd in 2026 — global price competition on actuators
is not winnable. What's left is an India-specific gap: non-Chinese
provenance, open firmware, indigenous-content procurement eligibility, local
support. The humanoid (v0) becomes a credibility/proof artefact, not the
business; a ₹5,000 phone-brained desktop kit becomes the community/fleet/data
layer, distributed through India's existing Atal Tinkering Labs network
(10,000+ already have 3D printers and ₹20L grants, documented as underused).

This also **superseded the original gating logic**. The prior entry's "2-week
guerrilla demand test before any chassis/control R&D" gate was the founder's
initially rejected recommendation, then reinstated informally by this revision
— the new plan uses milestone-based gates instead (month 10 / 13 / 18, see
`docs/18-month-plan.md`), not a pre-build demand test. This journal is the
record of that change; treat the earlier entry's "Next step" as superseded,
not deleted — the reasoning in it (OpenBot's non-commercialization, the
unsolved bipedal-control risk) still holds and carried forward into the new
plan's risk register.

Repo scaffolding was added by an AI coding session (Claude Code) at the
founder's request to "consolidate everything and develop this project."
Deliberately did **not** create any placeholder CAD, wiring diagrams, or
firmware — the build order in `docs/v0-build-spec.md` starts with physical,
measured steps (one servo on a bench; weigh the printed leg before ordering
more servos), and inventing engineering artifacts ahead of that would
misrepresent guesses as data. `cad/`, `electronics/`, `software/` are empty
except for READMEs stating what's supposed to land there.

**Why (if it failed):** n/a — planning/documentation entry, not a build
attempt.

**Open questions this raises:**
- Beachhead customer is still unresolved: defence/government procurement vs.
  education vs. export, per the comprehensive report's open decisions section.
  This is now flagged as possibly the most important open question in the
  whole plan.
- Product name is still blank.
- First actuator SKU torque rating (15 Nm vs 35 Nm) not yet decided (report
  leans 15 Nm).
- Full-time vs. part-time founder commitment not yet decided — changes every
  downstream timeline by ~50% and cuts Q5 (replication phase) if part-time.
- ToddlerBot's hardware license (non-commercial?) and Berkeley Humanoid
  Lite's license were both flagged as unverified in the build spec — neither
  has actually been checked yet.
- Whether "the mechanism worked for OpenBot and it still died" (a prior-art
  warning already on record) applies just as hard to the actuator thesis, not
  only the humanoid thesis, hasn't been separately stress-tested.

**Next step:**
Per `docs/18-month-plan.md` Q1: close a torque loop on a gimbal motor with an
MA732 encoder and STM32G4 (M1, targeted month 2) — this is the first real
hardware step and the next entry in this journal should report its actual
result, not a plan. Incorporation (Pvt Ltd, not OPC) and T-Works access are
parallel Q1 items with no build dependency. Verify the two open-source
hardware licenses (ToddlerBot, Berkeley Humanoid Lite) before any CAD reuse.
