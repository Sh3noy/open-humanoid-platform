# M1 bench joint

Documentation of a **planned** build. Nothing here has been ordered, wired,
flashed or measured yet. Every number marked "verify" is a claim from a
datasheet or vendor page that must be checked against the real part before it
is trusted. Per the repo's `CLAUDE.md`, this folder deliberately holds no
firmware, CAD or wiring artefacts; those appear only after the step that
produces them is done on real hardware, and land in `software/`, `cad/` and
`electronics/`.

Companion files: [BOM.md](BOM.md) (what to order), [LAB.md](LAB.md) (what the
lab needs).

## What M1 is

Milestone M1 of `docs/18-month-plan.md`: **a torque loop closes on a gimbal
motor** (target: month 2). It is the first step of build-order item 7 in
`docs/v0-build-spec.md` ("custom joint bench-proven"), done before any robot
exists. It ships as a **video of the motor holding position while a hand
pushes on it**.

## Why this joint

The one custom joint (gimbal motor + magnetic encoder + STM32G4 + FOC
firmware, CAN-FD later) is the seed of the actuator product line. Everything
else in v0 is scaffolding. M1 answers the cheapest first question: can we
close a current (torque) loop and a position loop on a gimbal motor we bought
in India, with a controller board we did not design? A stranger following the
repo must be able to repeat it, so every step below is written to be
reproducible, and dead ends go in `research/rd-journal.md`.

## Chosen stack (from the 2026-09-25 parts scout; see BOM.md for evidence)

| Role | Part | Why |
|---|---|---|
| Motor | GB2208 gimbal motor (Robokits) | Named in the v0 spec; sold in India |
| Encoder | MT6701 magnetic module with magnet | MA732 breakouts have no Indian listing found; MT6701 has SimpleFOC support |
| Controller | ST B-G431B-ESC1 | STM32G431, gate driver, current sense and ST-LINK on one board; no custom PCB for M1 |
| Firmware | SimpleFOC (MIT) | Documented on this exact board; fastest route to the video. moteus is the later reference (see the journal entry) |

## Done criterion

M1 is done when all of these are recorded, with the video as the artefact:

1. Closed-loop position hold: the shaft holds a commanded angle while a hand
   applies pressure, and returns to it on release.
2. The torque (current) loop is demonstrably closed: commanded current versus
   measured current logged, with the shaft blocked.
3. The setup is written up well enough to repeat: wiring, pin map, tuned
   gains, encoder alignment result, all in the repo.

Not part of M1: the "tracks commanded torque within 10%" and "holds under a
1 kg-cm load" checks in the v0 "done means" list. Those belong to the later
bench-proven step; M1 only records what is actually measured.

## Build plan

Do the steps in order and stop at any step that surprises you. Log every
surprise, failure and dead end in `research/rd-journal.md`.

### 0. Before power
- Read the B-G431B-ESC1 user manual and datasheet; write down the supply
  range, current rating, connector pinout and jumper/solder-bridge settings
  actually printed there. (The scout's "6-28 V" is unverified.)
- Confirm the motor's pole-pair count from its datasheet or by counting
  magnets. SimpleFOC needs it; 7 is only a typical value for this class.
- Measure phase-to-phase resistance with a multimeter and record it.

### 1. Unboxing and inspection
Check for shipping damage, note the motor label, the encoder module revision
and the magnet (diametrically magnetised, size per the encoder datasheet).
Photograph everything for the journal.

### 2. Mechanical mount
Fix the motor so it cannot move under hand pressure (clamp or printed bracket;
a printed mount needs a measured design, not a guessed one). Fix the encoder
board on the axis of the shaft, magnet centred and at the air gap the MT6701
datasheet specifies. Misalignment corrupts the angle reading, so check
alignment before anything else.

### 3. Wiring (bench supply OFF)
- Motor phases to the three phase terminals of the ESC1.
- Encoder to the ESC1 using the interface the board actually exposes (the
  scout lists I2C/SSI/ABZ/PWM on the module; pick per the SimpleFOC MT6701
  thread and the ESC1 pinout, and record the choice).
- Bench PSU to the board's supply input through the current limit, set well
  below the motor's rating (see Safety).
- Keep the motor phase wires short and the PSU leads twisted.

### 4. First contact and flashing
- Power the board from USB only first (ST-LINK enumerates, no motor power).
- Flash a blink or serial "hello" through the toolchain (PlatformIO or Arduino
  with STM32 support, as used in the SimpleFOC ESC1 threads) to prove the
  toolchain and ST-LINK path before any motor code.
- Flashing from a laptop or PC is the plan of record; see LAB.md for what the
  Android tablet can and cannot do.

### 5. Encoder-only test
Motor unpowered, turn the shaft by hand and read the angle over serial.
Expect a smooth 0 to 360 degree sweep, no jumps, direction sign known. Fix the
mount if not.

### 6. Current-sense sanity, then encoder alignment
- With the motor disconnected or unpowered, check the current-sense offset
  reads near zero.
- Run SimpleFOC's alignment (`initFOC`) at a very low voltage limit and record
  the electrical zero offset and detected direction. If direction or pole
  pairs are wrong, the motor will shudder or run away: cut power and re-check
  step 0.

### 7. Open-loop spin
Voltage/velocity open-loop at a low voltage limit. The shaft should turn
smoothly and quietly. This proves phases, driver and supply, without the
encoder. Log the voltage at which it turns and the current drawn.

### 8. Closed-loop position
Enable FOC with the encoder. Start with a low voltage limit, low proportional
gain, no derivative. Tune upward in small steps. Record each gain set that
works and each that oscillates.

### 9. Torque / current loop
Switch to torque control using the measured phase currents (the ESC1 has
shunt sensing; whether the SimpleFOC current-sense path on this board works at
the needed quality is one of the things M1 tests). With the shaft blocked
mechanically, command a current ramp and log commanded versus measured. Do
not block the shaft while the supply limit is high.

### 10. Hand-pressure test and video
Run closed-loop position hold at a modest current limit. Push the shaft by
hand, release, repeat. Record a video that shows the setup, the serial plot or
log, and the hand test. Commit the gains, wiring notes and log alongside it.

## Safety notes
- **Current limit first.** Set the bench PSU current limit before connecting
  the board, low (start around the low hundreds of mA and raise gradually).
  A gimbal motor is high-resistance, so it does not need much current, but a
  wrong pole-pair count or direction can run away. Keep a hand near the PSU
  switch and use the software current/voltage limit too.
- **Magnet alignment.** A misplaced magnet gives a wrong angle and a
  positive-feedback runaway. Verify the angle reading in step 5 before ever
  enabling FOC. Keep the magnet and encoder away from steel screws and tools.
- **Heat.** High current holding a fixed position heats a gimbal motor
  quickly. Keep the hold current low, touch-check the motor and the driver
  chips, and stop if too hot to hold a finger on. Never leave it powered
  unattended.
- **Electrical.** Never connect or disconnect motor phases with the supply on.
  Use a supply within the board's verified rating. Wear eye protection; keep
  hair and loose wires away from the rotating shaft (the printed shaft
  magnet holder can detach).
- **ESD.** Handle bare boards on an ESD mat or by the edges.

## Open items before starting
- Confirm every "verify" spec against ST and vendor datasheets.
- Decide the budget overrun (see BOM.md).
- Choose the flashing machine (LAB.md).
