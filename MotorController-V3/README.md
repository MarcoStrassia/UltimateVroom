# MotorController-V3

This is the newest board (pushed 2026-11-10). It is a new design, not an edit of `MotorController-Rebuild/`.

It is a 4-layer dual-motor FOC controller for the Boosted Board V1: one STM32G474, two DRV8300 gate drivers, twelve 100 V MOSFETs, low-side current sensing on every phase, and hall inputs for both motors.

**It is routed and passes KiCad's checks, but it is not ready to order.** The biggest reason is size: it does not fit the stock ESC footprint yet. The full list is under "Known problems".

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

## What has been checked

- KiCad DRC: 0 errors, 0 unconnected, schematic and board match. 24 warnings, all `hole_to_hole` (vias drilled very close together or on top of each other).
- KiCad ERC: 0 errors. 31 warnings (wire ends off grid, and library symbols that differ from the copies in the schematic).

Nothing here has been built or powered. No person has reviewed the layout yet.

## Known problems

**1. It is too big and the mounting holes are in the wrong place.**
The board is 92.4 x 100 mm with holes on a roughly 84 x 70 mm pattern. Measured from photos of a real V1 ESC, the stock board is about 74 x 85 mm with screws about 67.6 x 59 mm apart. Those photo numbers are good to about 1 mm, so the real board needs calipers: outline width and height, each screw centre from one corner, and the hole diameter. The next version has to be laid out again at the real size.

**2. Some high-current copper is narrower than it should be.**
The target is 13 A continuous with a 10 C rise (IPC-2221, 2 oz outer copper), which needs about 5.2 mm of width. Narrowest point on each path that falls short:

| Net | Narrowest | Carries about |
|---|---|---|
| Motor 1 phase A | 3.5 mm, bottom | 9.8 A |
| Motor 1 phase B | 3.75 mm, bottom | 10.3 A |
| Motor 1 phase C | 3.0 mm, bottom | 8.8 A |
| Motor 1 phase A low side | 5.0 mm, top | 12.7 A |
| Motor 2 phase A | 4.0 mm, bottom | 10.8 A |
| Motor 2 phase C | 3.0 mm bottom + 0.75 mm top | 12.0 A |

The necks are short, so a bench prototype at low current is fine, but they should be widened before running at full load. The estimate for where the battery bus and ground enter the inner planes near the FETs also comes out low (about 5 A per FET by the same rule). That estimate is conservative for a plane, but it has not been checked properly.

**3. The signal routing is messy.**
The PWM, hall and UART lines change layers far more often than they need to (65 extra layer changes). The cause is that the MCU pins were assigned before the parts were placed, so the PWM lines leave the MCU in a different order than the gate driver inputs. Six signal runs go through the In2 plane to avoid cutting the phase pours on the bottom: `M1_INH_A`, `M2_INH_A`, `M2_INH_B`, `M2_HALL_B`, `UART_TX`, and the motor 1 hall A/B input lines. They work, but they are slots in a power plane. The next version should fix the pin assignment first.

**4. Things to know before writing firmware.**
- UART TX is on PB11 and RX on PA3. That pair is LPUART1, not USART2.
- The two motors use the two advanced timers, TIM1 and TIM8. The pin map is in the schematic; check it against the datasheet before relying on it.
- There is no Bluetooth module on this board. The UART header is the only link for a remote.

**5. Not checked at all.**
Thermal behaviour, the fuse against two 30 A bursts, gate resistor values against ringing, the bootstrap and dead-time settings, and whether the flat bulk caps and the heatsink fit the enclosure.

## What is next

Measure the stock ESC with calipers, then redo the layout at the real size with the MCU pins assigned after placement, phase copper at full width, and the motor FETs along the edges next to the phase pads the way the stock board does it.
