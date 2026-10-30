# Boosted Board V1 ESC: measurement notes

These are estimates scaled from photos of the stock ESC, not caliper measurements yet. Confirm the board width and height and the pad pitch with calipers (checklist at the end).

Photo frame: upright photo, full-resolution pixels, origin top-left, x to the right, y down. The board rectangle is X 1101 to 2045, Y 1498 to 2614, so W = 944 px and H = 1116 px. Fractions below use x = 0 at the left edge (the battery/motor-lead side) and y = 0 at the top edge.

Annotated figure: `docs/boosted-v1-esc-reference.png` (photo overlay on the left, normalised board frame on the right).

## 1. Dimensions

| Dimension | Estimate | Method | Confidence |
|---|---|---|---|
| Board width | 84 mm (range 77 to 94) | 944 px at central scale 11.2 px/mm | low to medium |
| Board height | 100 mm (range 92 to 112) | 1116 px at central scale 11.2 px/mm | low to medium |
| Aspect H/W | 1.18 (1.16 to 1.20) | pixel ratio, independent of scale | medium |
| Scale (a) caps, L = 50 mm | 11.2 px/mm (11.0 to 11.4) | cap length 560 px | medium-low |
| Scale (b) red phase wire | 10.0 to 10.9 px/mm (central 10.4) | wire OD 60 px over 5.5 to 6.0 mm | low (OD assumed) |
| Scale (c) deck | 11.6 to 12.2 px/mm | local deck width about 2500 px over 205 to 216 mm | low to medium (deck width at the ESC station assumed) |
| Central scale | 11.2 px/mm (range 10.0 to 12.2) | median of the three estimates above | low to medium |

Cap hypotheses, checked against the photo:
- 10 mm dia x 50 mm: rejected. Cap diameter is about 208 px, which gives 20.8 px/mm and a board width of about 45 mm, inconsistent with the wire and deck scales.
- 12.5 mm dia x 50 mm: rejected for the same reason. It gives 16.6 px/mm and a board width of about 57 mm.
- Visible cap length to diameter ratio is about 2.7 (560 px over 208 px). A 50 mm length with the central scale implies a cap diameter of about 18.6 mm. Treat this as a photo inference, not a part spec.

Deck: the bamboo edges at x of about 1550 px sit at about y 830 (top) and y 3330 (bottom), so the local width is about 2500 px. The 216 mm figure is the Loaded Vanguard widest point, and the ESC sits near the deck centreline. Perspective and the lack of a known station make this roughly plus or minus 8 percent.

Phase terminal pitch: centre-to-centre spacing is 258 to 272 px, which is about 23.5 mm at the central scale (range 21 to 25 mm).

## 2. Connection points

Fractions are of board width (x) and height (y), from the upright photo.

| Name | x | y | Count | Notes |
|---|---|---|---|---|
| Phase terminal A (top edge, labelled A) | 0.18 | 0.08 | 1 | Solder blob, black lead |
| Phase terminal B (top edge, labelled B) | 0.47 | 0.05 | 1 | Solder blob, black lead |
| Phase terminal C (top edge, labelled C) | 0.75 | 0.04 | 1 | Solder blob, red lead |
| Phase terminal D (bottom edge) | 0.24 | 0.96 | 1 | Black lead, blue tie |
| Phase terminal E (bottom edge) | 0.52 | 0.96 | 1 | Black lead |
| Phase terminal F (bottom edge) | 0.80 | 0.96 | 1 | Red lead |
| Left-side thick leads (battery or motor?) | 0.05 | 0.36 | multiple | Thick red and black leads run left through the yellow glue toward the motor housing. Not confirmed as battery input. |
| Blue small parts on black wires (possibly hall connectors) | 0.26 / 0.29 | 0.13 / 0.90 | 2 | Blue cylinders with cable ties. Identity not confirmed. |
| Thin multi-colour hall bundle | about 0.05 to 0.10 | about 0.22 and 0.80 | 2 | Green, red, yellow, black wires in yellow glue, left side |
| White multi-pin header | 0.96 | 0.32 (span about 0.23 to 0.41) | 1 | Right edge, JST-style housing. Pin count not counted. |
| Bluetooth module with flex antenna | 0.96 | 0.49 (span about 0.43 to 0.55) | 1 | Beige module plus teal flex strip with ANT8030-01A label on the right edge. Mid-right edge, not a corner. |
| SMD LED cluster | 0.97 to 0.99 | 0.84 to 0.90 | 5 | Lower-right corner beside the BR screw. Video frame shows four yellow-green LEDs and one red LED. |
| Gold-pad SMD stack (below BR screw) | 0.99 | 0.91 (span about 0.87 to 0.94) | about 4 to 5 | Stacked parts labelled R2-UD and similar. Likely resistors; not verified. |
| Capacitor, upper | 0.63 | 0.41 | 1 | Lies along the board x axis, centre of the cap |
| Capacitor, lower (sticker side) | 0.63 | 0.54 | 1 | Same orientation, labelled ESCD1 sticker |

Mounting screws on the board (4 visible, all Phillips-head, as seen):

| Name | x | y |
|---|---|---|
| TL | 0.02 | 0.17 |
| BL | 0.06 | 0.87 |
| TR | 0.93 | 0.14 |
| BR | 0.97 | 0.83 |

Aluminium plate features, outside the board outline (upright px, not board-relative):
- Four plate bolt holes with bolt heads: about (1400, 1112), (1587, 1100), (1475, 3050), (1675, 3062).
- Two small plate screws: about (1487, 1037) and (1562, 3150).

## 3. Silkscreen and printed text

Read from the close-up photos. Confidence is marked. Nothing here is checked against a schematic.

Confirmed:
- `ESCD1-SY1506-009-0502` on the lower cap sticker.
- `105°C` on the cap sleeve, printed upside down.
- `CE` and `FC` marks on the caps.
- `Designed for` and `46V 30A/ch` in the top-right corner.
- `SN:` in a box, top-right corner.
- `A`, `B`, `C` beside the top terminals.
- `ANT8030-01A` on the antenna label, rotated (medium confidence on the exact reading).

Partial or uncertain (low confidence):
- `3V3` near the bottom-right (the leading "+" is not clear).
- `Q11A`, `Q11B`, `Q11C`-like fragments beside the top terminals.
- `C9nC`, `C3nC`, `C40`-like fragments on the right side.
- `R2-UD` and `R31`, `R3x`, `R4x`-like labels on the gold SMD stack.

Not found: `Q1hF`, `D2hF`, `+5V`, `VG`, `R22`, `C36`. These were not legible in any close-up, so they are unconfirmed.

## 4. Calipers checklist

- Board width (left to right edge, at mid-height), and board height (top to bottom), each measured twice.
- Board corners: note whether they are square or chamfered.
- Pad pitch of the three top terminals (A to B, B to C) and the three bottom terminals (D to E, E to F).
- Hole positions: the four board screw centres measured from the top-left corner (in mm), and the spacing between TL and TR and between BL and BR.
- Cap outside diameter and length, to confirm the 50 mm length and the diameter.
- Phase wire outside diameter (red), to confirm the 5.5 to 6.0 mm assumption.
- Heights above the board: top of the caps, top of the white header, top of the BT module.
