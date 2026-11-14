# MotorController-V3

This is the newest board (layout finished 2026-11-13). It is a new design, not an edit of `MotorController-Rebuild/`.

It is a 4-layer dual-motor FOC controller for the Boosted Board V1: one STM32G474, two DRV8300 gate drivers, twelve 100 V MOSFETs, low-side current sensing on every phase, and hall inputs for both motors.

**The layout is finished at its current size: it is fully routed, passes KiCad's checks, and every high-current path now meets the 13 A copper target. It still does not fit the stock ESC footprint**, so it is a bench board, not a drop-in. That and the smaller open items are under "Known problems".

## How to open it

1. On the GitHub page, click the green **Code** button, then **Download ZIP**, and unzip it. (Or `git pull` if you already cloned the repo.)
2. Install KiCad 10 if you don't have it.
3. Open `MotorController-V3/MotorController-V3.kicad_pro`. That opens the project; from there open the schematic and the PCB.

If you just want to look without installing anything, open the files in `previews/`:

| File | What it is |
|---|---|
| `previews/schematic.pdf` | The whole schematic |
| `previews/board-top.png`, `board-bottom.png` | 3D renders of both sides |
| `previews/layer-F.Cu.svg`, `layer-In1.Cu.svg`, `layer-In2.Cu.svg`, `layer-B.Cu.svg` | Each copper layer (open in a browser) |

## What is on it

| Part | Choice |
|---|---|
| MCU | STM32G474RET6, LQFP-64, 24 MHz crystal |
| Gate drivers | 2 x DRV8300UDPWR, one per motor |
| MOSFETs | 12 x SiR512DP (100 V, PowerPAK SO-8) |
| Current sense | 2 mOhm low-side shunt on every phase into INA240A1 amplifiers |
| Supplies | LM5164 buck to 12 V for the gate drivers, AP63203 buck to 3.3 V, TLV803E reset supervisor |
| Input | 40 A 72 VDC fuse, SMCJ48A TVS, 2 x 1500 uF 80 V bulk caps lying flat |
| Connections | Flat solder pads for battery and the six motor phases, one 5-pin JST-SH hall connector per motor, SWD header, UART header, three status LEDs, test points on the PWM lines |

Layers: top = parts and signals, In1 = ground plane, In2 = battery bus plane (3.3 V plane under the MCU), bottom = phase pours and signals. Outer copper is planned as 2 oz, inner as 1 oz. All parts are on the top side.

The board is sized for a 12S A123 pack (43.2 V charged) and about 13 A continuous, 30 A burst per motor.

## What changed on 2026-11-13

- The six half-bridges were re-rowed so each phase pad sits next to its FET pair. Phase current now runs on top and bottom copper in parallel and no longer squeezes past the shunt ground vias.
- Every signal was routed again after that move. The PWM lines run as two ordered buses on the bottom layer (motor 1 north-west from the MCU, motor 2 west under the MCU), so they reach the gate drivers in pin order without crossing.
- Layer changes on signal nets went from 62 to 14, vias from 814 to 513, track segments from 1696 to 1254.
- All routing is at 0, 45 or 90 degrees. The earlier version had 41 any-angle segments.
- Small part moves: the motor 2 phase A sense filter cap (C45) now sits at the MCU pin, the hall A pull-up (R56) sits next to its series resistor, R57/R58 are turned around, the PWM and brake test points are grouped next to the gate drivers and the MCU, and the U5 sense amplifier is wired like U6 and U7 so its ground pin is no longer boxed in.
- The schematic only changed in three footprint names (the flat solder pads now carry stitching holes).

## What has been checked

- KiCad DRC with schematic parity, zones refilled: 0 errors, 0 warnings, 0 unconnected, schematic and board match.
- KiCad ERC: 0 errors. 31 warnings (wire ends off grid, and library symbols that differ from the copies in the schematic), the same as before.
- Copper width on every high-current path against 13 A continuous, 10 C rise (IPC-2221, 2 oz outer and 1 oz inner copper). Every FET, shunt and connector now gets at least 13 A. The table is under "Copper widths" below.
- Gate drive: all 12 gate traces run next to their return, the longest is 32.0 mm of track (25.8 mm pad to pad).
- No signal track cuts a phase, bus or ground pour into two pieces.
- Gerbers, drill files, the position file and the BOM export from KiCad without errors.

Nothing here has been built or powered. No person has reviewed the layout yet.

### Copper widths

Narrowest point of each path and what the path carries with its layers in parallel:

| Net | Narrowest copper | Weakest part gets |
|---|---|---|
| Battery bus (VBUS) | 5.0 mm top, plus the In2 plane | 14.6 A (Q9) |
| Ground return | 2.75 mm bottom plus 8.25 mm on In1 | 13.8 A (R32) |
| Battery input (VBAT) | 13.0 mm top, 11.5 mm bottom | 48.6 A (fuse) |
| Motor 1 phase A / B / C | 5.25 mm top (A also 5.5 mm bottom) | 20.8 / 13.2 / 13.2 A |
| Motor 2 phase A / B / C | 5.0 mm top plus 5.5 mm bottom / 5.25 mm / 5.25 mm | 20.3 / 13.2 / 13.2 A |
| Low-side nodes A / B / C (both motors) | 5.0 mm top + 4.0 mm bottom / 3.5 + 3.75 mm / 5.0 + 4.25 mm | 18.4 / 13.6 / 18.4 A |

The previous version had four phase paths between 8.8 and 12.2 A and a battery bus estimate of 4.5 A per FET by the same rule. The margins on phases B and C (13.2 A) and on ground (13.8 A) are thin. They meet the rule, they do not beat it by much.

## Known problems

**1. It is too big and the mounting holes are in the wrong place.** This is the one thing that still stops it being a drop-in board.
The board is 92.4 x 100 mm with holes on a roughly 84 x 70 mm pattern. Measured from photos of a real V1 ESC, the stock board is about 74 x 85 mm with screws about 67.6 x 59 mm apart. Those photo numbers are good to about 1 mm, so the real board needs calipers: outline width and height, each screw centre from one corner, and the hole diameter. The size did not change in this update. Fitting the stock outline means laying the board out again.

**2. Signal runs inside the inner planes.**
To keep signals from cutting the phase and ground pours on the bottom, some of them run on the inner layers, always north-south so they sit along the current flow instead of across it:

- In the battery bus plane (In2): the three motor 1 hall lines and a +3V3 feed along the west edge (about 30 mm and 42 mm), the two phase A low-side gate traces (27 mm each), two short +12V crossings and two short +3V3 crossings.
- In the 3.3 V plane under the MCU (In2): the three motor 2 low-side PWM taps to their test points (10 mm each) and 8 mm of the reset line.
- In the ground plane (In1): the two phase B low-side gate traces, 15.5 mm each.
- The rest of the In2 routing (reset, motor 1 phase A/B current sense, motor 2 brake input, one +3V3 feed) is in the area north of the MCU where In2 has no plane, so it cuts nothing.

That is more inner-layer track than the previous version (about 215 mm in the bus plane against 123 mm, plus the two gate traces in the ground plane, which are new). The bus and ground still pass the copper check with these slots in place, but a cleaner board would keep In1 unbroken. The root cause is unchanged: the MCU pins were assigned before the parts were placed, so motor 1 and motor 2 PWM, sense and hall pins are interleaved around the package.

**3. Gate traces are long.**
Re-rowing the FETs moved some of them away from their driver. The longest gate trace is now 32.0 mm (phase C high side, both motors), against 17.3 mm before. Each one runs beside its return, and the limit set for this layout was 26 mm pad to pad and 33 mm of track, which holds. Gate resistor values should be checked on the bench for ringing.

**4. Bootstrap caps for phase B.**
The return side of C13 and C26 reaches its phase node through an 18 mm, 0.2 mm wide trace. It works electrically, but it should be shortened in the next layout.

**5. Current sense inputs share the ground net.**
Every INA240 IN- pin is on the GND net in the schematic. The layout keeps each IN- trace on its own path to the shunt pad (no vias or pours touching it away from the shunt), but a cleaner fix is to give each IN- its own net with a net tie at the shunt.

**6. Things to know before writing firmware.**
- UART TX is on PB11 and RX on PA3. That pair is LPUART1, not USART2.
- The two motors use the two advanced timers, TIM1 and TIM8. The pin map is in the schematic; check it against the datasheet before relying on it.
- There is no Bluetooth module on this board. The UART header is the only link for a remote.

**7. Small things.**
- R1's reference label is on the fabrication layer, not the silkscreen. There was no free spot for it between the phase pads.
- UART RX still changes layers twice more than it needs to.

**8. Not checked at all.**
Thermal behaviour, the fuse against two 30 A bursts, gate resistor values against ringing, the bootstrap and dead-time settings, and whether the flat bulk caps and the heatsink fit the enclosure.

## What is next

Measure the stock ESC with calipers, then redo the layout at the real size with the MCU pins assigned after placement, so the PWM, sense and hall lines leave the MCU in the order the drivers need and the inner planes can stay unbroken.
