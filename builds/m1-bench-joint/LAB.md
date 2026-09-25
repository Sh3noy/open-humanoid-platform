# Lab needs for M1 and the next steps

**None of the prices below were verified**: the parts scout priced only the
electronic parts, and no page for any tool was read. Every price here is
**E (estimate)** or "not priced". Get real quotes before buying. Where the lab
already owns something, skip it. The M1 bill of materials is in
[BOM.md](BOM.md).

## Must-have for M1

| Item | Why | Price |
|---|---|---|
| Current-limited bench power supply, adjustable voltage, roughly 0-30 V / 5 A | Runs the board with a hard current limit; the main safety tool | Not priced, real quote needed |
| Digital multimeter | Phase resistance, continuity, supply checks | Not priced |
| Soldering station (temperature-controlled), solder, flux | Attach motor leads and headers | Not priced |
| Wire strippers and cutters, heat-shrink, 22-24 AWG silicone wire | Motor and supply wiring | Not priced |
| Small hex driver set and precision screwdrivers (M2/M3) | Motor, mount and encoder screws | Not priced |
| Digital calipers | Measure motor, magnet and air gap before designing a mount | Not priced |
| ESD mat or wrist strap | Bare boards | Not priced |
| Safety glasses | Spinning shaft, magnet holder | Not priced |
| USB cable for the ST-LINK on the board (check the connector type on the real board) | Flashing and serial | Not priced |
| Way to make a motor mount: a 3D printer, or T-Works access | Fix the motor and encoder | T-Works membership is in the 18-month plan; price not researched |
| Laptop or PC for flashing | See below | Not priced |

### Flashing machine: laptop or PC versus the Android tablet
- **Plan of record: a laptop or PC.** The SimpleFOC ESC1 threads use the
  Arduino/PlatformIO toolchain with the onboard ST-LINK, which is
  well-trodden on Windows, macOS and Linux.
- **Android tablet (Termux/proot): not verified, do not depend on it.** Things
  that are known: the tablet has no GUI or browser preview in this setup, and
  the proot container is not the whole machine. Things not tested: USB OTG
  passthrough of the ST-LINK into Termux or proot, `platformio` or `openocd`
  running on aarch64 here, and serial monitor access. The tablet can still
  edit and read these docs and the journal. If flashing from the tablet is
  wanted, test it as a separate experiment and log the result, pass or fail.

## Consumables (M1)
Solder and flux, heat-shrink assortment, wire, M2/M3 screws and standoffs,
cable ties, electrical tape, isopropyl alcohol. Also spare filament if
printing the mount. All **E**, not priced. Roughly Rs 500-1,000 is a guess.

## Optional for M1
- Oscilloscope (or a cheap USB logic analyser) to look at phase voltages and
  encoder signals when debugging. Not needed to close the loop, since
  SimpleFOC can stream angle and current over serial. Not priced.
- Thermal camera or an IR thermometer for the heat checks. Not priced; a
  fingertip check is the fallback (see Safety in README.md).
- A spare of the encoder module and a second magnet.

## Later, not needed for M1
- CAN-FD transceiver breakout and a USB-CAN-FD adapter, for the CAN-FD
  interface milestone.
- Higher-current supply or 48 V supply and a torque sensor or simple dyno,
  for the actuator (~15 Nm) and the M4 100-hour test.
- Second and further gimbal motors and ESC1 boards for the leg (M2).
- Force gauge or a known weight on a measured lever arm, to test the
  "holds 1 kg-cm" and "torque within 10%" criteria in the v0 done-means list.
- Filament and printer access for the leg print (weigh it before ordering
  more servos, per the 18-month plan).
- Fire-safe storage and a charger for LiPo, once battery power is used.

## Decisions for the founder
- Which laptop or PC will flash and log.
- Printer versus T-Works for the mount.
- Which PSU to buy, once priced from a real cart.
