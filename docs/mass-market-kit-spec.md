# Mass-Market Kit — Spec

**A ~30 cm desktop humanoid torso that runs on your old phone. Target retail ₹4,999.**

Companion to the v0 prototype, not a replacement. v0 proves the actuators; this builds the fleet.

---

## Design rules

1. **The phone is the brain.** No onboard SBC. A docked Android phone supplies camera, mic, speaker, display, IMU, WiFi, NPU and backup battery. The robot is a body for hardware the user already owns.
2. **Nothing costs more than ₹500.** If a single component breaks that ceiling, redesign around it.
3. **One fastener size.** M3 screws and nuts throughout — available in every hardware shop in India.
4. **No ball bearings.** Printed bushings and M3 screws as pins.
5. **Under 15 hours total print time.** Print time is the real cost, not filament.
6. **Buildable with a screwdriver.** No soldering required in the base kit. No crimping. No calibration rig.
7. **Works with zero internet.** Cloud is optional, never required.

## Configuration

| | |
|---|---|
| Height | 300 mm seated/desktop torso |
| Mass | ~700 g excluding phone |
| DOF | 12 |
| Power | 5 V 5 A mains adapter. No battery. |
| Structure | PLA, ~250 g. Cardboard variant available. |

**Joint layout:** neck pan + tilt (2) · shoulder pitch + roll per arm (4) · shoulder yaw per arm (2) · elbow per arm (2) · gripper per arm (2).

No legs, no walking, no waist. Arms reach, point, gesture and lift ~50 g.

## Actuation

- **4 × ~1 Nm standard servos** (MG996R class) — shoulder pitch and roll
- **8 × ~0.2 Nm micro servos** (SG90 / MG90S class) — everything else

Driven by a PCA9685 16-channel PWM board off the ESP32.

**Feedback hack:** each servo's internal potentiometer wiper is tapped with a single wire to an ESP32 ADC input, giving joint position readback that hobby servos don't normally expose. Pre-wired in the kit so the user never solders. This is what makes closed-loop behaviour possible at this price.

## Bill of materials

| Item | ₹ |
|---|---|
| 4 × standard servos | 880 |
| 8 × micro servos | 560 |
| ESP32 dev board | 350 |
| PCA9685 servo driver | 150 |
| 5 V 5 A SMPS adapter | 300 |
| Filament (~250 g) | 180 |
| M3 hardware, wiring, phone cradle | 150 |
| **BOM total** | **~2,570** |

Street prices, Indian suppliers, quantity one. Expect 25–35% lower at 500 units.

## Product tiers

| Tier | What ships | ₹ |
|---|---|---|
| **0 — Files** | CAD, BOM, firmware, app. Source everything yourself. | Free |
| **1 — Electronics envelope** | Servos, ESP32, driver, harness, adapter. You print or cut the body. | 1,999 |
| **2 — Full kit** | Everything, printed parts included. | 4,999 |
| **3 — Cardboard kit** | Electronics + die-cut corrugated body. No printer needed. | 2,499 |
| **4 — School pack** | 10 × Tier 1 + teacher guide + curriculum | 17,999 |

Tier 1 is the flagship. Shipping electronics is cheap and robust; shipping printed plastic across India is neither.

## Software

- **Firmware (ESP32):** servo control, pot readback, serial/BLE protocol. Apache-2.0.
- **App (Android):** face rendering on screen, camera, wake word, speech in and out, behaviour scripting. Open source, on F-Droid.
- **Programming:** block-based for beginners, Python over WiFi for the rest. The block editor is what makes it usable in Class 6.
- **Languages:** Hindi, Telugu, Tamil, Bengali, Marathi at launch via Bhashini. This is a real moat — no imported kit will do it.

## Route to market

Over 10,000 Atal Tinkering Labs exist today, with 50,000 more rolling out over
five years. Every one is already equipped with a 3D printer, microcontroller
boards and sensors, and each carries a ₹20 lakh grant. The programme's known
weakness is underuse — labs lack teacher guidance and good projects.

The printers are bought. The budget exists. The gap is a project worth running.

**Sequence:** ship Tier 0 files free → seed 20 ATLs in Telangana with Tier 1 → publish student builds → apply for ATL vendor listing → national.

## Honest limitations

- Does not walk. It is a torso on a desk.
- Hobby servos: audible, jittery, no torque sensing, gear wear within 12–24 months of heavy use.
- Not a research platform. No sim-to-real transfer, no policy learning. That stays on v0.
- Not a household assistant. It is a teaching object and a community-building object.

## What it's really for

Every kit is a builder who now understands robot kinematics, a node that can later buy a real actuator, and a potential contributor to the standard. The ₹5,000 robot is not the business. It is how the business gets ten thousand people who care.
