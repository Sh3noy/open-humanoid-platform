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
