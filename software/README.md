# software/

Everything that runs on the robot or drives it: the ROS 2 stack and sim-to-real
pipeline for v0, the FOC firmware for the custom actuator, and the ESP32
firmware + Android app for the mass-market kit.

**Empty.** Per the build order in `../docs/v0-build-spec.md`, software work
starts with "one servo on the bench, read and write over the bus" — nothing
here should be scaffolded ahead of that, since a control stack or sim model
written against no real hardware is just placeholder code pretending to be
tested. When that work starts, the planned pieces are:

- **`v0/`** — ROS 2 workspace; MuJoCo/Isaac Lab sim model built to match the
  physical leg's measured response (not estimated dimensions — see the "weigh
  it before ordering" note in the build spec); the RL walking policy;
  sim-to-real transfer code. Forked from ToddlerBot's codebase (MIT) — its
  hardware license needs verifying separately before any CAD reuse, per the
  build spec.
- **`actuator-firmware/`** — FOC firmware for the STM32G4 + MA732 encoder
  joint, forked from moteus or ODrive, CAN-FD protocol implementing the
  published joint interface (spec not written until a working joint exists —
  see `../docs/18-month-plan.md` Q4).
- **`kit-firmware/`** — ESP32 firmware: servo control, potentiometer-wiper
  position readback, serial/BLE protocol. Apache-2.0.
- **`kit-app/`** — Android app: face rendering, camera, wake word, speech,
  behaviour scripting, block-based + Python programming interfaces. Open
  source, intended for F-Droid.

Intended license: Apache-2.0 for everything here (see `../LICENSE`) — not yet
applied per-file since none of these components exist.
