# v0 Prototype — Build Spec

**A ~45 cm 3D-printed bipedal humanoid.** Small enough to fall over safely, complex enough that walking is a real problem.

---

## Physical

| | |
|---|---|
| Height | 450 mm standing |
| Mass | 2.8–3.2 kg including battery |
| Structure | 3D-printed. PETG for shells and non-structural parts; PA-CF or PC for hip, knee and shoulder brackets |
| Payload | 200 g per hand (reach and hold, not manipulate) |
| Runtime | 30–45 min active |

Everything is printable on a 256 mm bed. No part requires machining, and no part requires a printer we don't already have access to.

## Degrees of freedom — 22 total

| Group | DOF | Notes |
|---|---|---|
| Legs | 12 | 6 per leg: hip yaw, hip roll, hip pitch, knee pitch, ankle pitch, ankle roll |
| Arms | 8 | 4 per arm: shoulder pitch, shoulder roll, shoulder yaw, elbow pitch |
| Neck | 2 | pan, tilt |

Legs get the full six because dropping ankle roll makes lateral balance dramatically harder and walking is the whole point. Arms get four — enough to reach and counterbalance, not enough for fine manipulation. Wrists, grippers and a waist joint are deliberately deferred.

## Actuation

Two torque classes, sized to load rather than bought uniformly:

- **~5 Nm class × 6** — hip pitch, hip roll, knee (both legs). These carry body weight in single support.
- **~3 Nm class × 16** — everything else.

Serial-bus smart servos (Feetech STS / Waveshare ST family), daisy-chained on a single half-duplex TTL pair. Position, load and temperature come back over the same wire.

**Known limitation:** one shared bus at ~1 Mbps across 22 servos caps the practical control rate at roughly 100–200 Hz. Fine for a 50 Hz walking policy. It is also precisely the constraint that CAN-FD on our own joints removes later — worth feeling first-hand.

### The one custom joint

One actuator is built in-house rather than bought: gimbal motor (GB2208 class) + MA732 magnetic encoder + STM32G4 + FOC firmware + CAN-FD out.

Prove it on the bench first. Once it holds position and tracks torque reliably, swap it into **shoulder yaw** — a low-load joint where failure doesn't drop the robot. Everything else on this machine is scaffolding; this joint is the seed of the product line.

## Electronics

- **Compute:** Raspberry Pi 5 (8 GB) onboard. Mount pattern also accepts a Jetson Orin Nano for when vision goes on.
- **Sensing:** 9-axis IMU (BNO085 or ICM-42688), one fisheye camera, foot contact switches under each sole. An optional off-board room sense is described below; the robot does not carry it.
- **Power:** 3S LiPo, 2200–3000 mAh. Direct to servo rail, buck to 5 V for the Pi.
- **Off-board:** RTX 6000 Ada workstation for MuJoCo / Isaac Lab policy training. The robot runs inference only.

### Ambient room sense (off-board, optional)

Two or three ESP32-S3 boards fixed around the test space capture WiFi channel state information (CSI) — the amplitude and phase of the radio link between them, per subcarrier. A person moving or breathing in that space disturbs it. Software turns the disturbance into coarse room events, and the robot only subscribes to those events. **No CSI hardware rides on the robot** — see the limitation below and the R&D journal (2026-09-20).

**What it adds.** The IMU and foot switches measure the robot's own body; the fisheye needs both light and line of sight. The room sense is the one input that says "someone is in the room" or "the room has gone still" with no line of sight, in the dark, and with no camera in the room. In v0 nothing acts on it (autonomy is not in v0): the events are logged alongside each test run.

**How events reach the robot.** Nodes stream CSI over WiFi to a host on the same network — the training workstation or a spare Pi. The host publishes events (presence, motion and, if it survives testing, a breathing-rate estimate with a confidence value) over MQTT or UDP, and a small bridge republishes them as ROS 2 topics on the Pi 5's existing WiFi link. Where the signal processing runs — on the node or on the host — is undecided until it can be measured. Espressif's `esp-csi` (Apache-2.0) documents three ways to source CSI: from a router, between two nodes, or from a dedicated transmitter.

**Cost.** ESP32-S3 dev boards were listed at ₹684–1,089 each by Indian retailers on 2026-09-20 (Quartz Components, Robocraze), so two to three nodes is roughly ₹1,400–3,300 in boards, before power supplies and mounts. This sits outside the ~₹71,000 budget below and adds no onboard compute. The "$9 node" in circulation is a price for a board, not for a working sense.

**Known limitation:** the published evidence on ESP32-class hardware supports coarse presence and motion detection, and — in small controlled studies (the one ESP32 heart-rate result found used seven seated participants) — breathing and heart rate. It does not support pose: the ESP32-S3 is a single-antenna, 2.4 GHz-only part, while CMU's WiFi-DensePose used two routers with 3×3 antenna pairs, and its average precision fell from 43.5 to 27.3 when tested in a room layout it had not seen. Detectability also depends on where the person stands and which way they face. None of it has been measured in our room, so treat every capability here as a hypothesis until it is logged in the journal. The room sense is not a safety input and must never gate a walking trial or an e-stop.

**Known limitation, onboard:** a node on the walking robot would see its own motion swamp the human signal, and compensating for that is an open research question — one recent paper handles it for human-proximity detection on mobile robots, and nothing found covers a biped or vital signs. It is deliberately not a v0 item.

## Software

ROS 2 for plumbing · MuJoCo or Isaac Lab for sim · RL walking policy trained in sim, transferred zero-shot · LeRobot for later imitation work · teleop rig for data collection.

Fork ToddlerBot's codebase (MIT) for the low-level control and sim-to-real pipeline. **Confirmed 2026-08-30 (via GitHub, see R&D journal): ToddlerBot's hardware design — Onshape document, STL files — is CC BY-NC-SA 4.0, non-commercial only.** Its CAD cannot be reused for a commercial product; redraw the mechanical design from scratch, using the code and published dimensions as reference only.

Berkeley Humanoid Lite is the better reference for the actuator/leg side (sub-$5,000, 3D-printed gearboxes). **Confirmed 2026-08-30: code is MIT; hardware/other assets are CC BY-SA 4.0** — commercial use is permitted (attribution + share-alike required). Prefer this repo over ToddlerBot wherever both cover the same part, since its license doesn't block the actuator business.

## Done means

1. Stands unassisted for 60 seconds.
2. Walks 2 m in a straight line without falling.
3. Survives 20 falls from standing with no structural repair.
4. A policy trained in sim transfers to hardware with no hand-tuning of gains.
5. The custom joint tracks commanded torque within 10% and holds position under a 1 kg-cm load.
6. **Someone else builds one from our repo without asking us a question.** This is the real test.

## Explicitly not in v0

Hands or dexterous manipulation · autonomy or navigation · speech · any cloud dependency · cosmetic shell · hot-swap battery · WiFi sensing mounted on the robot.

## Build order

1. One servo on the bench, read and write over the bus.
2. One leg on a test rig, tracking a trajectory.
3. Both legs plus pelvis, tethered to a gantry, balancing in place.
4. Untethered standing.
5. Walking.
6. Arms and head.
7. Custom joint bench-proven, then swapped in.

Do not print a torso until step 3 works.

## Budget

| Item | ₹ |
|---|---|
| 6 × ~5 Nm servos | 15,000 |
| 16 × ~3 Nm servos | 24,000 |
| Filament, bearings, fasteners | 8,000 |
| Pi 5, camera, IMU | 12,000 |
| Battery, wiring, bus board, PSU | 6,000 |
| Custom joint (motor, encoder, MCU, PCB) | 6,000 |
| **Total** | **~71,000** |

Excludes a 3D printer. If one isn't on hand, T-Works is cheaper than buying for a single build.
