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
- **Sensing:** 9-axis IMU (BNO085 or ICM-42688), one fisheye camera, foot contact switches under each sole.
- **Power:** 3S LiPo, 2200–3000 mAh. Direct to servo rail, buck to 5 V for the Pi.
- **Off-board:** RTX 6000 Ada workstation for MuJoCo / Isaac Lab policy training. The robot runs inference only.

## Software

ROS 2 for plumbing · MuJoCo or Isaac Lab for sim · RL walking policy trained in sim, transferred zero-shot · LeRobot for later imitation work · teleop rig for data collection.

Fork ToddlerBot's codebase (MIT) for the low-level control and sim-to-real pipeline. Verify its **hardware** licence before reusing mechanical design — it appears to be non-commercial, in which case the CAD gets redrawn.

## Done means

1. Stands unassisted for 60 seconds.
2. Walks 2 m in a straight line without falling.
3. Survives 20 falls from standing with no structural repair.
4. A policy trained in sim transfers to hardware with no hand-tuning of gains.
5. The custom joint tracks commanded torque within 10% and holds position under a 1 kg-cm load.
6. **Someone else builds one from our repo without asking us a question.** This is the real test.

## Explicitly not in v0

Hands or dexterous manipulation · autonomy or navigation · speech · any cloud dependency · cosmetic shell · hot-swap battery.

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
