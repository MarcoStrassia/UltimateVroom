# UltimateVroom

UltimateVroom is a dual-motor brushless controller for a Boosted Board V1. Marco and I are building it to replace the dead stock ESC.

It runs from a 12S A123 26650 pack, which is 39.6 V nominal and 43.2 V charged, and drives two motors. An STM32F103 does the control, IRS2104 gate drivers switch CSD19532 FETs, and the board is 4 layers.

The KiCad project is in `MotorController-Rebuild/`. GitHub doesn't render KiCad files, so clone the repo and open `MotorController-Rebuild/MotorController-Rebuild.kicad_pro` in KiCad 10. `MotorController-Rebuild/docs/` has our spec PDF, design notes, and notes and photos of the stock ESC. `MotorController-Rebuild/review/current/previews/` has images of the board and a PDF of the schematic. `earlier-schematic/` is Marco's first schematic.

The board is routed. ERC has 0 errors and 7 warnings (four-way junctions). DRC has 0 errors and 0 unconnected items, and the schematic matches the board.

Still open: motor current limits, a thermal check, firmware, the Bluetooth remote, and fitting the board into the Boosted V1 ESC footprint, which is about 84 x 100 mm with six phase pads.

Patrick Liu and Marco Strassia, UCSB
