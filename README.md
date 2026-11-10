# UltimateVroom

UltimateVroom is a dual-motor brushless controller for a Boosted Board V1. Marco and I are building it to replace the dead stock ESC.

It runs from a 12S A123 26650 pack, which is 39.6 V nominal and 43.2 V charged, and drives two motors. An STM32F103 does the control, IRS2104 gate drivers switch CSD19532 FETs, and the board is 4 layers.

## Newest board: `MotorController-V3/`

The newest board is in the `MotorController-V3/` folder (added 2026-11-10). Start with `MotorController-V3/README.md`: it says which file to open, what has been checked, and what is still wrong with it. Short version: it is a new 4-layer design around an STM32G474 and DRV8300 drivers, it is routed and passes KiCad's checks, and it is not ready to order because it is still bigger than the stock ESC.

To get the files without using git: click the green **Code** button on the GitHub page, then **Download ZIP**.

## Earlier board: `MotorController-Rebuild/`

Everything below describes the earlier board.

The KiCad project is in `MotorController-Rebuild/`. GitHub doesn't render KiCad files, so clone the repo and open `MotorController-Rebuild/MotorController-Rebuild.kicad_pro` in KiCad 10. `MotorController-Rebuild/docs/` has our spec PDF, design notes, and notes and photos of the stock ESC. `MotorController-Rebuild/review/current/previews/` has images of the board and a PDF of the schematic. `earlier-schematic/` is Marco's first schematic.

The board is routed. ERC has 0 errors and 7 warnings (four-way junctions). DRC has 0 errors and 0 unconnected items, and the schematic matches the board.

Still open: motor current limits, a thermal check, firmware, the Bluetooth remote, and fitting the board into the Boosted V1 ESC footprint, which is about 84 x 100 mm with six phase pads.

Patrick Liu and Marco Strassia, UCSB
