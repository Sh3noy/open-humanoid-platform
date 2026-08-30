# cad/

Source CAD and exported STL/STEP files for the v0 prototype chassis and the
mass-market kit body.

**Empty.** Per the build order in `../docs/v0-build-spec.md`, CAD work starts
at step 2 ("one leg on a test rig") and a torso isn't printed until step 3
works — no chassis design exists yet, and nothing here should be fabricated
ahead of that physical groundwork. When parts start landing:

- Source files (whatever CAD tool is chosen — see the R&D journal for the
  decision and why) alongside exported `.stl` for printing and `.step` for
  interchange.
- One subfolder per assembly (`leg/`, `arm/`, `torso/`, `kit-body/`, …).
- Intended license: CERN-OHL-W (see `../README.md`) — not yet applied.
