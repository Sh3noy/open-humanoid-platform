# 18-Month Plan

**Month 0 = 2026-09-25**, the date the entity and grant decisions below were made. Month N ends on the 25th of the calendar month N months later (Month 10 = 2027-07-25, Month 13 = 2027-10-25, Month 18 = 2028-03-25). The gates in this plan are measured from this date.

**Assumes:** one to two people, funded by grants and the founder's own money, Hyderabad. If this is part-time alongside other work, add roughly 50% to every timeline and cut Q5 entirely.

---

## The spine

Six milestones. Everything else is support work.

| # | Milestone | By |
|---|---|---|
| **M1** | A torque loop closes on a gimbal motor | Month 2 |
| **M2** | One 6-DOF leg tracks a trajectory on a rig | Month 5 |
| **M3** | The robot walks 2 m unassisted | Month 9 |
| **M4** | Custom joint survives 100 hours under load | Month 12 |
| **M5** | Someone else builds one from the repo | Month 14 |
| **M6** | First actuators sold | Month 17 |

M3 is the one that unlocks everything else — grants, contributors, attention. Protect it.

---

## Q1 · Months 1–3 — Bench

- Close a torque loop on a gimbal motor with MA732 encoder and STM32G4. **(M1)**
- Run open-humanoid as a venture of the founder's One Person Company (OPC), whose registration is in progress. Register for DPIIT recognition once the OPC exists. Trade-off, stated plainly: an OPC has a single member and cannot take equity investors. If outside equity is ever needed, the OPC can be converted to a Private Limited company, and this venture can move with it.
- Prepare and submit the NIDHI-PRAYAS application (see Grant calendar).
- T-Works membership for printer and workshop access.
- Repo public from week one, while it's still bad. Licence files: Apache-2.0 is in place; CERN-OHL-W (hardware) and CC BY-SA 4.0 (docs) still to add, with the licence text in hand.
- Print one leg. **Weigh it before ordering the remaining servos** — the leg torque class depends on actual mass, not estimated.

**Ships:** video of a motor holding position against hand pressure.

## Q2 · Months 4–6 — One leg

- 6-DOF leg on a test rig, tracking commanded trajectories. **(M2)**
- MuJoCo model that matches the physical leg's response.
- Custom joint rev 2: add the planetary stage, characterise on a simple dyno.
- NIDHI-PRAYAS decision expected; if approved, work moves onto grant milestones.

**Ships:** side-by-side video, sim leg and real leg doing the same motion.

## Q3 · Months 7–9 — It walks

- Both legs plus pelvis, tethered to a gantry, balancing in place.
- Untethered standing, then walking. RL policy trained on the RTX 6000, zero-shot transfer.
- Arms and head last.
- **(M3)** Publish the walk video, the CAD, and the full BOM the same day.

**Ships:** the video that makes everything after this easier.

## Q4 · Months 10–12 — Reliability and the spec

- Custom joint rev 3–4. Run it 100 hours under cyclic load. **(M4)**
- Swap it into shoulder yaw on the robot.
- Publish **joint interface spec v0.1** — mounting pattern, 48 V CAN-FD, protocol. Not before now: a spec with no working implementation behind it is noise.
- Complete build documentation to the standard that a stranger can follow.
- Apply to the DST indigenous-supply-chain challenge (figures unverified).

**Ships:** an interface spec and a joint that doesn't die.

## Q5 · Months 13–15 — Replication and the kit

- Get **three external people** to build v0 from the repo. Pay their BOM if needed — it is cheaper than any marketing. **(M5)**
- Design and prototype the ₹5,000 mass-market kit. This is fast now: the CAD, app and servo-bus work already exist.
- 20 Tier-1 kits into Telangana ATLs.
- Build 20 units of the first actuator SKU — **15 Nm only**.

**Ships:** somebody else's robot walking, filmed by them.

## Q6 · Months 16–18 — Revenue

- Sell the first 10–20 actuators to Indian robotics teams and college labs. **(M6)**
- Kit Tier 1 on sale.
- Teleop data pipeline live; publish the first Indian-environment manipulation dataset.
- Raise: RDI scheme is now viable (TRL 4+ reached), or a seed round with revenue in hand.

**Ships:** an invoice.

---

## What is deliberately not happening

- **The three-SKU actuator line.** One SKU. The 5 Nm and 35 Nm versions are year three.
- **The full-size wheeled humanoid.** Year three.
- **The foundation or standards body.** Needs three external adopters first.
- **A community.** Expect five to twenty people by month 18, not five hundred. Design the contribution surface now; don't plan around it carrying weight.
- **The mass-market kit before M3.** It is easier and more fun than the hard work, which is exactly why it will eat the year if you let it start early.

## Budget

| | ₹ |
|---|---|
| v0 prototype including breakage and spares | 1,50,000 |
| Custom joint — 5 revisions plus PCB runs | 70,000 |
| 3D printer (or T-Works access instead) | 1,20,000 |
| Bench PSU, scope, torque test rig | 60,000 |
| Kit prototyping and first 20 units | 90,000 |
| First actuator batch (20 units) | 2,00,000 |
| Incorporation, compliance, misc | 50,000 |
| **Total, 18 months, excluding salary** | **~7,40,000** |

A NIDHI-PRAYAS grant (₹20 L via a PRAYAS Centre in the PC category, per the comprehensive report) would cover this budget with room to spare. Grant funding is a bet, not a given. Without it, the founder self-funds only the first two lines and the rest waits.

## Gates

Pre-commit to these now, while it's easy to be honest. The earlier "2-week demand test before any build" gate is retired (see the R&D journal, 2026-09-25).

**Gate 1 — Month 10.** If it isn't walking, the problem is scope, not skill. Cut the arms and neck, drop to 12 DOF, ship the walk.

**Gate 2 — Month 13.** If the custom joint can't survive 100 hours, stop building actuators. Buy them, and become a robot company instead. That is a real outcome, not a failure.

**Gate 3 — Month 18.** If nobody has bought an actuator, the component thesis is wrong for now. The kit and education business is the fallback — and given 10,000 ATLs with underused printers, it is a genuine business, not a consolation.

## Grant calendar

Track 1 (the hardware build) is gated on grant applications, not on a demand test. NIDHI-PRAYAS goes first because it does not need a registered company. The Startup India Seed Fund follows once the OPC is registered and DPIIT-recognised. TiHAN was dropped as a primary track on 2026-09-25: its startup call needs a registered, incubated startup and focuses on autonomous navigation, UAV, 6G and CPS, not actuators.

| When | What | Size |
|---|---|---|
| Q1 | Submit NIDHI-PRAYAS via a PRAYAS Centre; T-Works membership | ₹20 L (PC) / ₹40 L (APC) |
| Q1 | DPIIT recognition (after OPC registration) | — |
| Q2 | Startup India Seed Fund Scheme, via an approved incubator (needs registered OPC; check current terms) | Unverified |
| Q4 | DST indigenous supply chain | Unverified |
| Q4 | DSIR recognition — customs duty exemption on imported research equipment | ongoing benefit |
| Q6 | RDI scheme (needs TRL 4+), or seed | ₹1 Cr+ |

DSIR is worth doing early despite the paperwork: it directly reduces the landed cost of every imported motor, encoder and magnet you buy for the next three years.
