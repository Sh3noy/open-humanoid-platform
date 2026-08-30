# Open Humanoid Robotics — Comprehensive Report

**Hyderabad, India · August 2026 · Pre-formation**

---

## 1. Summary

Build an **open robot joint standard for India**, prove it with a small humanoid, and seed a builder community with a cheap kit. Three artefacts, one thesis: India cannot make robot actuators, and that gap is worth more than another humanoid.

The honest framing, stated up front: this is a **sovereignty and openness play, not a cost play**. You will not beat Chinese actuator pricing. What you can own is provenance, transparency and support.

---

## 2. The thesis, and where it holds

**Where it holds.** Every Indian humanoid company is a customer at the joint layer, not a competitor. Cosine in Bengaluru explicitly adopted an "actuator first" strategy for exactly this reason. The Indian open-hardware robotics lane is genuinely empty — Vanar, Cosine, Ati, Muks, iHub, Addverb and Axiom are all closed. India imports the large majority of core robotics components, and a genuine harmonic drive for a 100 Nm actuator starts near $3,000 before duty.

**Where it doesn't.** I initially described actuators as an unfilled product gap. That was wrong. CubeMars ships roughly 6 million units a year across 100+ models, contributed to MIT's Mini Cheetah, and IPO'd in 2026. Their AK80-9 is a 9:1 integrated QDD joint with 22 Nm peak, motor, gearbox, encoder and driver in one unit, with MIT control mode. Their AKE line achieves 9 arcminutes backlash and 52 N·m/kg.

**So the product gap is not global — it is Indian.** At 20 units against their 6 million, price competition is not available. What survives:

| Advantage | Real? |
|---|---|
| Non-Chinese provenance | Yes — the FCC Covered List makes this worth a premium |
| Open firmware, no black box | Yes — CubeMars is closed |
| Local support, short lead times | Yes, but a service advantage, not a moat |
| Indigenous-content procurement | Yes — defence and government mandate it |
| Cheaper than imports | **No** |

**Consequence for go-to-market:** first buyers are likely defence-adjacent, government-linked, or export customers constrained by the FCC rules — not price-sensitive Indian startups, who will rationally buy CubeMars.

---

## 3. What gets built — three layers

| Layer | Purpose | Timeline |
|---|---|---|
| **v0 prototype** — 45 cm bipedal humanoid | Research platform, credibility, the video | Months 1–9 |
| **Actuator** — one SKU, ~15 Nm | The product, the revenue | Months 4–17 |
| **Kit** — ~₹5,000 desktop torso | Community, fleet, data | Months 13+ |

They are not versions of each other. The v0 proves the technology, the actuator sells, the kit recruits.

---

## 4. v0 prototype

**~45 cm, ~3 kg, 22 DOF, 3D-printed, bipedal.**

- **Legs 12** (6 per leg: hip yaw/roll/pitch, knee pitch, ankle pitch/roll) · **Arms 8** (4 per arm) · **Neck 2**
- Full 6 DOF per leg — dropping ankle roll makes lateral balance dramatically harder, and walking is the point
- **~5 Nm class × 6** for hip pitch/roll and knee; **~3 Nm class × 16** elsewhere
- Serial-bus smart servos on one half-duplex TTL pair
- Raspberry Pi 5 onboard; RTX 6000 Ada (or subsidised cloud) for policy training
- ROS 2 · MuJoCo/Isaac Lab · LeRobot · **~₹71,000 in parts**

**Why small:** at this scale torque is trivial, falls cost nothing, and iteration is cheap. ToddlerBot (0.56 m, 3.4 kg, 30 DOF, under $6,000 BOM) does cartwheels and its builders report it almost never breaks. Note this **inverts** the full-scale advice — at 1.5 m, skip legs; at 45 cm, legs are the affordable part.

**Fork:** ToddlerBot's code (MIT) for control and sim-to-real; check its **hardware** licence before reusing CAD — it appears non-commercial. Berkeley Humanoid Lite (sub-$5,000, 3D-printed gearboxes) is the better reference for the actuator side — **verify its licence, which I never checked.**

**Done means:** stands 60 s · walks 2 m · survives 20 falls unrepaired · sim policy transfers with no hand-tuned gains · someone else builds one from the repo without asking a question.

---

## 5. The actuator

**Quasi-direct drive: large-diameter BLDC + single planetary stage, 8–9:1.**

The strategic reason for QDD over harmonic: it moves difficulty out of precision gear grinding — where India's machining base is thinnest — and into motor winding and control firmware, where it is strongest. It is also backdrivable, so inherently safer around people.

**Target the 20–60 Nm class** (shoulder, elbow, wrist). Leg joints need 200+ Nm and force you toward cycloidal. **One SKU at ~15 Nm** for the first 18 months. Three SKUs is a team's output, not one person's.

**Stack:** gimbal-class BLDC · MA732-class magnetic encoder · STM32G4 · FOC firmware forked from moteus or ODrive · CAN-FD out · aluminium housing.

**The standard is the actual product.** Publish an interface — mounting pattern, output flange, 48 V, CAN-FD, versioned open protocol. If Indian robotics teams design around your joint interface, you win on units you never manufacture. Arduino, not a product company. **Publish it only after a working joint exists**; a spec with no implementation is noise.

**Magnet exposure:** a 30 Nm joint carries ~150 g of NdFeB versus 1–3 kg in an EV traction motor. Ten thousand joints is under two tonnes. That's a stockpiling problem, not a supply-chain-killer. Keep a ferrite or reluctance fallback design in the repo.

---

## 6. The mass-market kit

**~30 cm desktop torso, 12 DOF, target retail ₹4,999, BOM ~₹2,570.**

The single move that makes the price work: **the phone is the brain.** A docked Android phone supplies camera, mic, speaker, display, IMU, WiFi, NPU and backup battery — hardware most Indian households already own, dead in a drawer. An ESP32 at ₹350 drives the servos. You are building a body for a computer people already have.

Other reductions: hobby servos with a potentiometer-wiper tap for position feedback (untested at manufacturing scale — **validate on three servos before committing the design**) · mains power, no LiPo · ship electronics, not plastic · a cardboard tier for anyone without a printer.

**Distribution:** over 10,000 Atal Tinkering Labs exist with 50,000 more rolling out over five years, each with a ₹20 lakh grant and 3D printers already installed — and a documented weakness of underuse from lack of teacher guidance and good projects. The printers are bought. The gap is a project worth running.

**Honest limits:** doesn't walk · hobby servos are noisy and wear out in 12–24 months · not a research platform · not a household assistant. It is a teaching object that builds a fleet.

---

## 7. Decentralisation, concretely

- **Manufacturing** — published BOM and CAD; certified fabricator network builds to spec; you hold the trademark and the certification mark
- **Compute** — each joint runs its own current and position loop on its own MCU over the bus. No central motor controller. Distributed by architecture
- **Governance** — a standards body, but only after three or four external companies adopt the interface. Not year one; open projects need a benevolent dictator for the first three years

---

## 8. Contribution model

The test is whether a contribution compounds or just forks.

**Compounds:** teleop demonstrations from Indian homes · translated build guides · city-level supplier lists · build logs of failure points · sim models · trained policies · servo drivers · gearbox designs · print profiles for cheap printers · Indic wake-word and TTS · certified fabricators.

**Doesn't:** body restyles, one-off variants, cooler heads. Welcome them; don't organise around them.

**The highest-value contribution needs no engineering.** Manipulation data from Indian kitchens — steel tumblers, dupattas, dal sorting — is the only asset in this plan that capital cannot shortcut. Figure and Unitree have vastly more money and zero access.

Two mechanisms worth copying: a **public ranked problem list** (RoboParty does this), and a **one-click data pipeline**. If uploading a demonstration takes more than record-and-sync, you get none.

**Realistic expectation:** five to twenty contributors by month 18, not five hundred. Contributors arrive after a working thing exists.

---

## 9. Plan — 18 months

| # | Milestone | By |
|---|---|---|
| M1 | Torque loop closes on a gimbal motor | Month 2 |
| M2 | One 6-DOF leg tracks a trajectory | Month 5 |
| M3 | Robot walks 2 m unassisted | Month 9 |
| M4 | Custom joint survives 100 h under load | Month 12 |
| M5 | Someone else builds one from the repo | Month 14 |
| M6 | First actuators sold | Month 17 |

**Q1** bench and one joint; incorporate; T-Works access; repo public while it's bad. **Q2** one leg on a rig; MuJoCo model matching hardware; apply PRAYAS. **Q3** walking, published same day as CAD and BOM. **Q4** joint reliability; publish interface spec v0.1; documentation. **Q5** three external builders; kit prototype; first 20 actuators. **Q6** revenue.

**Not happening:** three SKUs · the full-size wheeled humanoid · a foundation · a large community · the kit before M3 (it is easier and more fun than the hard work, which is why it will eat the year).

**Gates, pre-committed:**
- **Month 10** — not walking? Cut arms and neck, drop to 12 DOF, ship the walk.
- **Month 13** — joint can't survive 100 h? Buy actuators, become a robot company. A real outcome, not a failure.
- **Month 18** — nobody bought an actuator? The component thesis is wrong for now; the kit and education business is the fallback and it's genuine.

**Budget:** ~₹7,40,000 over 18 months excluding salary — prototype ₹1.5 L, joint revisions ₹70 k, printer ₹1.2 L (or T-Works), test equipment ₹60 k, kit ₹90 k, first actuator batch ₹2 L, compliance ₹50 k.

**The binding constraint is attention, not money.** ₹7.4 lakh is findable. Eighteen months of one person is not expandable.

---

## 10. Funding map

**Verified:**

| Source | Amount | Notes |
|---|---|---|
| **NIDHI-PRAYAS 2.0** | ₹20 L (PC) / ₹40 L (APC) | Grant, no equity. Physical products only, software excluded. Apply via a PRAYAS Centre, not DST directly. IP vests with you |
| **ARTPARK @ IISc** | varies | Best strategic fit — core robotics components are in their stated portfolio; backed by DST, Karnataka and the Ministry of Heavy Industries |
| **IHFC, IIT Delhi** | varies | Cobotics hub — human-adjacent robots, i.e. your backdrivable-joint safety argument |
| **IITM Pravartak** | varies | Charter area is sensors, networking, actuators and controls — literally your category |
| **T-Works** | subsidised access | Telangana govt, 78,000 sq ft at Raidurg. Prototyping service or trained equipment access. solutions@tworks.in. Also T-Works × HDFC Parivartan for hardware startups |
| **TiHAN, IIT Hyderabad** | ₹15–25 L | Autonomous navigation only — UAVs, ROVs, ground vehicles. **A humanoid joint is a stretch pitch.** I over-recommended this twice |
| **RDI Scheme** | ₹1 Cr+ | Needs TRL 4+. 3–4% loans, 12–15 year tenure, up to 50% of project cost |
| **DSIR recognition** | ongoing | Not a grant — customs duty exemption on imported research equipment. Cuts landed cost of every motor and encoder for years. Apply early |
| **IndiaAI compute** | ₹65–92/GPU-hr | 38,000+ GPUs, up to 40% subsidy vs ₹300–600 commercial. **Wrong tool for RL sim-to-real** (iterative debug loop, and 48 GB Ada suffices); right tool later for VLA fine-tuning |

**Unverified — treat as leads:** iDEX ₹1.5 crore · MeitY SAMRIDH · Genesis · T-Hub · TSIC. The ARTPARK ₹2 crore challenge figure is from a 2024 round.

**Sequencing:** pick two — a PRAYAS Centre and ARTPARK — plus T-Works because it's local. Grant writing is a real tax. Everything else waits until M3, when a walking robot makes every application easier to write and easier to approve.

**NM-ICPS was extended to December 2027.** Several hub programmes approach the end of their funded period; confirm call status with each hub directly, not via listing sites.

---

## 11. Regulatory gates

**BIS — the kit is a regulated product.** The Toys Quality Control Order (2020) requires the ISI mark on all toys sold in India. Electric toys fall under IS 15644; non-electric under IS 9873. Certification runs 3–6 months and ₹30,000–1,50,000. Penalties reach ₹5 lakh for first offences, up to two years imprisonment for repeat violations, plus seizure — and uncertified products cannot be sold on major e-commerce platforms **or used in government procurement**, which is exactly the ATL route.

The 5 V adapter falls separately under CRS. Watch Scheme-X for industrial machinery and electrical gear, with firm enforcement from 1 September 2026, which may touch the actuator line.

**Framing matters:** an educational/lab product sold to institutions sits differently from a toy sold to children. Decide with a compliance consultant *before* marketing.

**DPDP Act.** The dataset thesis means recording video inside Indian homes — personal data, with consent, retention and processing obligations. The entire data moat has a compliance layer attached.

---

## 12. Risk register

| Risk | Severity | Mitigation |
|---|---|---|
| **Commodity competition** — CubeMars at 6 M units/yr | High | Compete on provenance, openness, support. Never on price |
| **Capital** — Cosine ran actuator-first and stalled; Indian deep-tech investors won't fund pre-revenue | High | Grants and bootstrapping. Don't plan for VC |
| **Magnets** — India has no commercial-scale sintered NdFeB; REPM bids opened Aug 2026, two-year gestation to follow, so ~2029 | Medium | Joints are magnet-light. Stockpile; keep a ferrite fallback |
| **Prior art** — xTerra Robotics already builds QDD actuators in India, closed-source, for their own legged robots | Medium | Contact early: competitor, partner or customer |
| **Export** — FCC Covered List (28 July 2026) blocks new authorisations for **foreign-made** humanoids and quadrupeds, Indian included. Unitree certified its lineup weeks before | Medium | Bare components without radios are likely out of scope. Another argument for the joint layer over the robot |
| **BIS** — kit blocked from platforms and government procurement | Medium | Budget 3–6 months and ₹1 L; frame as educational equipment |
| **Solo founder** | High | Firmware hire by month 4 is on the critical path and currently unplanned |

---

## 13. Structure

**Pvt Ltd.** Open source is a licensing decision about code and CAD; incorporation is about who invoices, imports and hires. Red Hat and Canonical are ordinary for-profit companies.

**Not OPC** — a single-member company cannot take equity, and you will want to.

**Not Section 8 as well** — I recommended a dual entity earlier and that was over-engineered for one person. Two boards, two audits, no benefit until external contributors need IP held neutrally. Add later, or never.

**Licensing:** CERN-OHL-W hardware (commercial use permitted, changes flow back) · Apache-2.0 firmware · trademark on the name and certification mark.

---

## 14. Open decisions

1. **Name.** Still blank.
2. **Full-time or alongside other work?** If part-time, add ~50% to every timeline and cut Q5 outright — not a stretch of the same plan.
3. **First SKU: 15 Nm or 35 Nm?** Recommend 15 — middle of the range, lessons scale both directions.
4. **Beachhead customer** — given the commodity finding, defence/government procurement vs education vs export. This may be the most important unresolved question in the document.
5. **"Everyday purposes"** was your original brief. This report steers to components and education. Defensible, but it's a reframing — confirm it consciously.
6. **Compute** — subsidised IndiaAI instances with your own layer on top is a separate company, not an addition. A fork in the road.

---

## 15. Confidence

**Well-sourced (government or primary):** the magnet scheme timeline, the FCC Covered List action, IndiaAI compute pricing, NIDHI-PRAYAS 2.0 amounts, TiHAN's scope, T-Works, ATL numbers, BIS toy and CRS requirements, and NM-ICPS hub assignments.

**Single-source, verify before quoting:** the "India imports 90% of core robotics components" figure traces to one outlet whose startup coverage doesn't corroborate elsewhere. Direction is well-supported; don't put the number in a deck without a NITI Aayog or IFR citation.

**Not verified:** iDEX amounts, SAMRIDH, Genesis, Berkeley Humanoid Lite's licence, current ARTPARK call terms.

**Estimates, not quotes:** all BOM figures, servo prices and the ₹7.4 lakh budget. Re-price before committing.
