# v3 status: unfinished, do not fab

This is a work-in-progress snapshot of the v3 board from 2026-11-06. It is not routed. The finished board on `main` is still `MotorController-Rebuild/`.

Open `mc2.kicad_pro` in KiCad 10.

## Parts

STM32G474RET6, DRV8300UDPWR gate drivers, SIR512DP-T1-RE3 FETs (Q1-Q12), AP63203WU-7 for 3.3 V. The full list is in `mc2-BOM.csv`.

## Checks (KiCad 10.0.6, 2026-11-09)

| Check | Result |
| --- | --- |
| DRC errors | 0 |
| DRC warnings | 4 (2 hole-to-hole on VBUS vias, 2 silkscreen) |
| Unconnected items | 171 |
| Schematic parity | 0 |
| ERC errors | 0 |
| ERC warnings | 33 (21 off-grid endpoints, 12 library symbol mismatches) |

The biggest groups of unconnected items are +3V3 (51) and +12V (12).

## Progress

The board is being built one block at a time.

| Block | State |
| --- | --- |
| battery_input | Finished and reviewed |
| motor1_power_stage | Finished and reviewed |
| motor2_power_stage | In progress. Routed once, but never passed review, and its copper is not locked |
| gate_supply_12v | Not finished |
| current_sense | Not finished |
| supply_3v3_reset | Not finished |
| mcu_core | Not finished |
| ble_halls_io_mechanical | Not finished |

## Known problems

- **U11 may have the wrong pin map.** A review found the schematic symbol still using the TPS54202 pins although the part is now an AP63203. The fix belongs to the supply_3v3_reset block and has not been confirmed. Check this before trusting the 3.3 V rail.
- **Motor 1 gate paths use a relaxed rule.** They are allowed 2 vias instead of 1, with the 20 mm length limit kept, because 1-via routes did not fit with the current driver placement.
- **Some parts still need review.** Several parts in the unfinished blocks have no recorded design-in evidence, including U2/U3, the INA240 current sense amps and D14-D16.

Library tables here use KiCad's `${KICAD10_FOOTPRINT_DIR}` and `${KICAD10_SYMBOL_DIR}` in place of the original Mac install paths.
