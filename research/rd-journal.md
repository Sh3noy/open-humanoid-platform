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

---

## 2026-08-30 — Verified ToddlerBot and Berkeley Humanoid Lite licenses

**Status:** success

**What was tried:**
Checked both open-source reference projects' actual license files via the
GitHub API (`gh api repos/hshi74/toddlerbot`,
`gh api repos/HybridRobotics/Berkeley-Humanoid-Lite`, plus their READMEs) —
these had been sitting as unverified assumptions since the initial plan
revision earlier today, flagged in both the build spec and the comprehensive
report as "appears non-commercial" / "never checked."

**What happened:**
- **ToddlerBot:** codebase is MIT (confirmed via repo's `license` field and
  LICENSE file). Hardware design (Onshape document, STL files) is
  **CC BY-NC-SA 4.0** — non-commercial only, confirmed directly from the
  repo's README license section. The original guess was right: this CAD
  cannot be used in a commercial product. Code (control, sim-to-real) is
  still fair game to fork.
- **Berkeley Humanoid Lite:** codebase is MIT. Hardware/"other assets" are
  **CC BY-SA 4.0** — commercial use is permitted, attribution + share-alike
  required. This is the better hardware reference of the two specifically
  *because* its license doesn't block the actuator business — updated the
  build spec and comprehensive report to prefer it over ToddlerBot wherever
  both cover the same part (e.g. leg/gearbox design).
- Note: the two Berkeley Humanoid Lite submodule repos
  (`berkeley_humanoid_description`, `Berkeley-Humanoid-Lite-Lowlevel`) don't
  carry a GitHub-detected license field of their own; the main repo's README
  license statement was treated as covering them, but this wasn't checked
  file-by-file inside the submodules — worth a closer look before actually
  importing files from those specific submodules.

**Why (if it failed):** n/a — verification succeeded, no blockers found.

**Open questions this raises:**
- Confirm the submodule-level license coverage (see note above) before
  importing any specific file from `berkeley_humanoid_description` or
  `Berkeley-Humanoid-Lite-Lowlevel`.
- CC BY-SA 4.0 is share-alike — any derivative hardware design built from
  Berkeley Humanoid Lite's CAD must itself be released under a compatible
  share-alike license. Confirm this doesn't conflict with the project's own
  intended CERN-OHL-W (both are copyleft-flavored, but compatibility between
  a CC hardware license and CERN-OHL-W hasn't been checked).

**Next step:**
No blocker remains on the license front for starting hardware reference work.
Proceed to M1 (torque loop on a gimbal motor) per the existing Q1 plan; when
CAD work actually starts (build-spec step 2+), pull geometry/reference from
Berkeley Humanoid Lite first, ToddlerBot's code (not CAD) second.

---

## 2026-09-20 — WiFi CSI sensing: what checked out, and the off-board decision

**Status:** in-progress (pre-hardware; sourcing and scoping only — nothing
built, flashed or measured)

**What was tried:**
The founder supplied an architecture note for a WiFi-sensing perception stack:
ESP32-S3 boards deployed as a mesh capture WiFi Channel State Information
(CSI); quantised edge models turn it into semantic states (presence, breathing
rate, heart rate, falls, sleep) published over MQTT/UDP. The note attributes
this to an open-source project called RuView, cites an "8 KB 4-bit quantised
WiFi DensePose model" and "cryptographic attestation" of the sensor data, and
prices a node at "$9". Before any of it went into the specs, every claim that
could be checked was checked against primary sources — the GitHub API, the
papers' own PDFs, Hugging Face model cards, Indian retailer listings — in an AI
coding session (Claude Code). Numbers below were read at source. One fetch-tool
summary (a heart-rate paper's subject count and error figures) and one search
snippet (which hardware a breathing-rate paper used) were wrong when compared
with the paper text, and were discarded.

**Why this is interesting for this platform specifically:**
- **No camera in the room.** Nothing is imaged. (Not the same as "no personal
  data" — see the privacy item below.)
- **Works in the dark.** It is radio, not light; an ordinary fisheye camera
  needs both light and line of sight.
- **Cheap and India-sourceable.** ESP32-S3 dev boards were in stock at Indian
  retailers on 2026-09-20: Quartz Components ₹684 (listed as "ESP32E-N16R8" dual
  USB-C; its description says ESP32-S3-WROOM-1), Robocraze ₹899 (7Semi N8R8)
  and ₹1,089 (Seeed XIAO ESP32-S3).
- **Open tooling.** Espressif's `esp-csi` and ESP-IDF are both Apache-2.0, the
  same licence as our software.
- **It fits the kit.** The kit already carries an ESP32; Espressif documents a
  mode where one ESP32 and an ordinary router produce CSI.
- **It is the one input the robot's own senses cannot give:** "is someone in
  the room / has it gone still," with no line of sight and no light.

**What happened — what checked out:**
- **RuView exists.** `github.com/ruvnet/RuView`, MIT (LICENSE: "Copyright (c)
  2024 rUv"), created 2025-06-07, pushed 2026-09-20, ~94.5k stars, ~12.5k
  forks, 742 open issues on 2026-09-20. The main author (`ruvnet`, ~1,090
  contributions) is joined by a `claude` account (~105); the repo carries its
  own `CLAUDE.md` and agent scaffolding. Popularity is not validation. The
  README labels itself beta software and, to its credit, retracts some of its
  own numbers (below) — the README and Hugging Face card are a better guide to
  what works than the headline.
- **Espressif supports CSI across the chip family.** `esp-csi` (Apache-2.0)
  lists ESP32 / S2 / C3 / S3 / C5 / C6 / C61 and ships a `wifi_sensing_demo`
  for motion and presence. The ESP32-S3 is a 2.4 GHz-only, single-stream part
  (802.11 b/g/n, per its datasheet). RuView's README says its own DSP does not
  support the original ESP32 or the C3; that is a limit of RuView, not of the
  chip.
- **WiFi-to-DensePose is real research, with a narrow envelope.** Geng, Huang
  and De la Torre (CMU, arXiv 2301.00250): two TP-Link AC1750 routers
  (3 antennas each, so 3×3 antenna pairs), 30 subcarriers around 2.4 GHz, 100
  Hz. 16 spatial layouts (6 lab office, 10 classroom), 8 subjects in total,
  and labels that are pseudo ground truth from a camera-based DensePose
  network, not manual annotation. Same-layout AP 43.5 / AP@50 87.2, against
  84.7 / 94.4 for the image-based network. In a layout unseen in training, AP
  falls from 43.5 to 27.3. The authors list failure on three or more
  concurrent subjects and on rare poses, and say they are limited by the
  public training data.
- **Breathing and heart rate from cheap WiFi hardware appear in the
  literature, in small controlled studies.** VitalCSI (MDPI Sensors, doi
  10.3390/s26010225; abstract read): 15 healthy volunteers, a consumer access
  point plus a single-antenna Raspberry Pi, nasal-airflow reference, MAE 1.20
  breaths/min, r² = 0.93, over 6–33 breaths/min — and the paper itself says
  prior work is "limited to narrow respiratory ranges and small-scale
  validation." PulseFi (arXiv 2510.24744, a preprint; read from the PDF): on
  ESP32, two boards (one transmitter, one receiver, 80 Hz, 20 MHz, 64
  subcarriers), **7 participants sitting in a chair between the devices**, 1–3
  m separation, finger pulse oximeter as reference: heart-rate MAE 0.50 BPM
  with 5 s windows. Its split is a shuffled 64/16/20 window split; I did not
  see a held-out-participant test for the ESP32 data, and its ten-fold figure
  (15 s windows) is MAE 0.892 ± 0.40. Read this as "possible under favourable,
  still, seated conditions," not as a validated room sensor. Wang et al.
  (UbiComp 2016) is the standing reminder that respiration detection with
  commodity WiFi depends on where the person is and which way they face
  (Fresnel-zone model; I could not retrieve the abstract, so I cite only what
  its title asks).

**What happened — what did not check out, or only partly:**
1. **"An 8 KB 4-bit quantised WiFi DensePose model" — not as stated.** The 8 KB
   figure is the 4-bit variant (`model-q4.bin`) of a contrastive CSI *embedding
   encoder* with a 128-dim output (the v2 card describes an 8→64→128 network),
   shipped with a presence head in `ruvnet/wifi-densepose-pretrained`. It is
   not a pose model. It was trained self-supervised on **one overnight
   capture: one room, one sleeping person, two ESP32-S3 nodes, 6,063 feature
   frames**. Its original headline "100% presence accuracy" was retracted by
   the maintainers on the model card: 6,062 of 6,063 frames were labelled
   "present", so a constant "yes" scores 99.98%. The replacement figure, 82.3%
   held-out temporal-triplet accuracy, measures embedding quality — the card
   states it "measures embedding quality, not downstream presence/vitals
   accuracy (which needs multi-class, multi-room labelled data we don't yet
   have)." The README also says the quantised `.bin` files "still need a
   compatible reader," and that the published `model.safetensors` has a
   NUL-padded header the reference loader rejects (issue #1522, pending a
   corrected upload per the README).
2. **Pose from an ESP32 — not demonstrated.** RuView's on-device pose model
   scores PCK@20 = 3.0% (its own target is ≥ 35%) and its runtime path is a
   stub returning confidence 0. The 82.69% torso-PCK@20 figure is a separate
   model on the MM-Fi benchmark, captured on TP-Link N750 routers with the
   Atheros CSI Tool at **5 GHz, 3 antenna pairs × 114 subcarriers**. The ESP32-S3
   is 2.4 GHz and single-antenna; the RuView README itself says a single-antenna,
   56-subcarrier stream does not carry that spatial information and that live
   single-ESP32 pose should not be advertised. MM-Fi's own WiFi 3D-pose
   baseline is 186.9 mm MPJPE in its in-domain split and 367.8 mm when tested
   in an unseen environment.
3. **"Cryptographic attestation of the sensor data" — partly, and not where
   the note puts it.** The Ed25519 witness chain the README describes lives on
   the **Cognitum Seed**, a separate commercial device (a Pi Zero 2 W-based
   unit per ADR-066, listed at $131 on the model card), not on the $9 node.
   The design for a signed, staged chain from radio observation to decision
   (ADR-319, dated 2026-08-11) is marked "Accepted — initial implementation
   planned," and its own context says RuView "already has the pieces of a
   chain but not the chain itself." The README's "Witness attestation" row
   (`WITNESS-LOG-028`) is an audit of the repository's capabilities at one
   commit, performed by an automated three-agent Claude audit — an attestation
   about the repo, not about sensor data. I could not determine the Seed's own
   licence (its product page returned nothing usable).
4. **"$9 hardware" — true of a board, not of a working sense.** The README's
   own comparison is ~$54 for a 3–6 node mesh plus a router and ~$140 with a
   Seed. Indian listings are ₹684–1,089 per S3 board before power and mounts.
5. **Vital-sign accuracy in RuView — no validation found.** The README tables
   give 6–30 and 40–120 BPM ranges and a "±1 BPM" breathing figure in an
   applications table, but I found no comparison against a reference device in
   its `docs/benchmarks/` directory (pose, person-count, efficiency, home
   integration — nothing on vitals). Its own issues describe the problem: #593
   (closed) found linear statistics being applied to wrapped phase and feeding
   the heart-rate FFT, "probably the cause of the very jumpy vitals"; #485
   (open) describes the earlier heart-rate output as "varying ±15 bpm
   minute-to-minute"; #519 (closed) reports ghost persons and jumpy vitals on
   three ESP32-S3 nodes. The model card says health features are "for screening only, not medical
   diagnosis" and that breathing/HR are "less accurate during active movement."
   (Limits of this check: GitHub code search plus reading the benchmarks
   directory, not the whole repository.)
6. **"Through walls" — unverified.** RuView states ~5 m, "signal-dependent."
   I found no independent ESP32-S3 measurement. The platform does not rely on
   it.
7. **"Privacy" — narrower than it sounds.** No camera is true. But presence,
   breathing, heart rate and sleep state of people in their homes are still
   information about identifiable people. `docs/comprehensive-report-2026-08.md`
   already flags the DPDP Act for the video dataset; whether CSI-derived events
   fall under it was **not researched** and needs counsel. RuView's own
   re-identification experiment (WiFi-only cardiac+respiratory channels could
   not separate two people) is a single self-reported result, not a privacy
   guarantee.

**What happened — licences (checked the way ToddlerBot and Berkeley Humanoid
Lite were):**
- **RuView: MIT** (repo LICENSE; the Hugging Face model card metadata also says
  MIT for `wifi-densepose-pretrained`); **RuVector: MIT**. I did not audit
  `vendor/` or transitive dependencies, so "MIT" covers the repo's own code.
- **`ruvnet/wifi-densepose-mmfi-pose` — non-commercial.** Its model card
  declares CC BY-NC 4.0 and states "this model inherits the non-commercial
  license" from the MM-Fi dataset (which the MM-Fi paper also licenses CC
  BY-NC 4.0). The 82.69% "state of the art" number therefore belongs to
  weights we could not ship in a product.
- **ESPectre** (`francescopace/espectre`, the popular ESP32 CSI motion project):
  **GPL-3.0**, with a separate commercial licence offered. It cannot sit
  inside Apache-2.0 kit firmware.
- **`esp-csi` and ESP-IDF: Apache-2.0** — compatible with our software licence.
  I did not separately check the licence of the `esp_wifi_sensing` component
  the `wifi_sensing_demo` uses. Believed but **not checked**: Espressif's WiFi
  PHY/MAC firmware ships as precompiled binary libraries, which matters for
  any provenance claim this platform makes about the sense.
- **Cognitum Seed:** licence and openness **undetermined** (some
  `cognitum-one` repositories are MIT; the Seed itself was not established).
  Nothing here should depend on it.

**Why the off-board shape (decided 2026-09-20):**
The founder was told that a CSI node riding on a walking robot has its signal
dominated by the robot's own motion, and that compensating for self-motion is
an unsolved research problem. Everything the sourcing found is consistent with
taking the off-board shape: every published number above comes from fixed
transmitters and receivers, with fixed geometry and no moving receiver. So
WiFi CSI enters the platform two ways only — as an
**ambient room sense** the robot subscribes to (`docs/v0-build-spec.md`), and as
an optional, off-by-default feature of the mass-market kit, where an ESP32 is
already in the design (`docs/mass-market-kit-spec.md`). Both are
documentation; there is no code, BOM or firmware, and none belongs here until
a board is on a bench (`software/README.md`, `electronics/README.md`).

**The deferred onboard question, stated properly:**
A CSI measurement is the sum of every radio path between transmitter and
receiver. A person breathing perturbs one or a few of those paths by a small
amount; the receiver moving changes all of them. At 2.4 GHz the wavelength is
about 12.5 cm (c ÷ 2.4 GHz), and v0's done-means walk is 2 m — sixteen
wavelengths — so a receiver on a walking biped moves through many
full-wavelength phase rotations in the course of one trial, plus gait
vibration and motor noise on the same board. The human signal a stationary
node can find is not obviously recoverable underneath that. What is known:
- **CornerSense** (Proc. ACM IMWUT, published 2025-12-02; abstract read) says it
  is the first work to investigate WiFi sensing on mobile robots, that "the
  intertwined movement of the robot and nearby humans complicates the
  isolation of human-induced signal variations," and handles it with a PCA
  scheme that first extracts a reference path containing only the robot's
  motion, subtracts it, and extracts the human-reflected path from the
  residual. It reports 96% true-positive / 3% false-positive human-proximity
  detection across nine corners and four walking patterns, against 84% / 46%
  for a stationary-transceiver algorithm. That is a real result on a narrow
  task (binary proximity detection). I did not confirm what robot platform it
  used, and it does not address vital signs or a walking biped.
- **MSense** (MobiCom 2024; known from its published summary, not read in
  full) frames the same problem — the device must stay static and the person
  must stay still for fine-grained sensing — and solves it with mmWave radar
  and digital beamforming. Different sensor, and an antenna array; it does not
  transfer to a single-antenna ESP32.
- **No source found** for self-motion compensation on a biped, on an ESP32, or
  for breathing/heart rate from a moving receiver. This search was not
  exhaustive.

So the accurate statement is not "nobody has tried" but: *early, one
proximity-detection paper on mobile robots; nothing for a walking biped or for
vitals.* A hypothesis — untested, not a plan — is that the robot's IMU and
joint encoders know its own motion and could act as regressors to subtract it.
If this is ever pursued, the first measurement would be to record CSI with the
receiver on the leg test rig tracking a trajectory in an empty room, and see
how much of the CSI variance the robot's known motion explains against what a
person breathing 1–3 m away contributes. That is a research question, not a
build item, and it is not scheduled.

**Why (if it failed):** n/a for the decision itself. The pattern behind the
"did not check out" list is that the source project's headline claims outran
its own measurements, and its maintainers have partly corrected them in public
(the retracted presence figure, the labelled first-cut pose model). The
underlying physics and the published research are real; the packaged numbers in
the note are not evidence for our hardware.

**Open questions this raises:**
- Does presence/motion detection work at usable reliability in *our* room, with
  the robot rig, workshop equipment and people moving around it? Untested.
- Is breathing or heart rate obtainable from an ESP32-S3 mesh outside a
  controlled, seated-subject setup? The literature above says only "possible,
  small n."
- Does CSI capture coexist with the kit ESP32's WiFi link and servo loop at
  usable packet rates? Unmeasured.
- Do CSI-derived presence and vitals of people in homes count as personal data
  under the DPDP Act, and what consent does the kit need if the feature ships?
  Not researched; counsel question.
- Do trained weights inherit the non-commercial terms of their training
  dataset? Treated here as "yes until resolved" for the MM-Fi pose weights.
- What is the Cognitum Seed's licence, and is a signed evidence chain needed at
  all for a room sense whose events we only log in v0?
- The onboard self-motion question above.

**Next step:**
Nothing to build now. When an ESP32-S3 pair is on a bench and the room is
free, the first entry should be a measurement, not a plan: log CSI with the
room empty, with one person walking, seated, and breathing still, against a
manual log or PIR ground truth, using `esp-csi` (not RuView's numbers), and
record what it cannot detect. Until then this sense is a documented option:
it changes no done-means criterion, no milestone and no budget line in
`docs/18-month-plan.md`.

---

## 2026-09-25 — M1 bench joint: parts and firmware chosen (planned, not bought)

**Status:** in-progress

**What was tried:**
A desk-research parts scout for the M1 bench joint (plan in
`builds/m1-bench-joint/`). Web pages only; nothing ordered, nothing built,
no seller contacted. Several shop pages returned 403, so most prices come from
search snippets and are marked as such in `builds/m1-bench-joint/BOM.md`.

**What happened:**
Chosen kit: GB2208 gimbal motor (Robokits, Rs 2,474, page read), MT6701
magnetic encoder module with magnet (about Rs 300-500, estimate), ST
B-G431B-ESC1 (STM32G431 + driver + current sense + onboard ST-LINK, about
Rs 4,600 from a TME India snippet), SimpleFOC firmware (MIT). Total about
Rs 7,500 before bench PSU and mount, about Rs 1,500 over the Rs 6,000 custom-joint
budget line.
- **MT6701 over MA732:** the v0 spec names MA732, but no Indian listing for an
  MA732 breakout was found, while MT6701 modules are listed by Indian sellers
  and SimpleFOC has an MT6701 discussion and support. MT6701 is a listed
  alternative in the brief. Its accuracy on our motor is untested.
- **SimpleFOC for M1, moteus later:** SimpleFOC (MIT) has documented support
  for the B-G431B-ESC1 and is the quickest route to the hold-position video.
  moteus (Apache-2.0 per a snippet, to verify) targets mjbots hardware and
  fits the CAN-FD actuator plan better, but porting it to this board is real
  work (pin map, driver, current sense). Use moteus as the reference for the
  actuator protocol from M2 onward and record the port cost when it is tried.

**Why (if it failed):** n/a, nothing has been tried on hardware.

**Open questions this raises:**
- Accept the roughly Rs 1,500 overrun, or get a delivered Mouser/DigiKey
  quote first? The TME price conflicts with import snippets.
- B-G431B-ESC1 supply range, current rating and CAN transceiver type are from
  memory and unverified.
- Motor pole pairs and phase resistance are unconfirmed.
- Can the Android tablet flash the ST-LINK at all? Untested; laptop is the plan.
- Does the ESC1's current-sense path give usable torque-loop quality under
  SimpleFOC? This is what M1 tests.
- Bench PSU, mount and lab tools are unpriced (`builds/m1-bench-joint/LAB.md`).

**Next step:**
Verify the datasheet values, get real prices from a cart, decide the budget,
then order and follow the build plan in `builds/m1-bench-joint/README.md`.
