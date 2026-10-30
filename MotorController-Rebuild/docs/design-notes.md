# Design notes

Target: 12S1P A123 26650 pack (39.6 V nominal, 43.2 V charged) and the two stock Boosted Board V1 motors.

## Power stage

Each motor has its own inverter: three IRS2104 half-bridge drivers and six CSD19532Q5B MOSFETs, with bootstrap supplies, gate resistors, snubbers, DC ceramics and three phase terminals. The two channels share the reset and emergency-stop signals. PWM, enables and current sense are separate.

STM32F103 pins: motor 1 uses TIM1 on PA8/PA9/PA10 with enables on PB12/PB13/PB14. Motor 2 uses TIM3 on PA6/PA7/PB0 with enables on PB6/PB7/PB8 and current feedback on PB1 (ADC9). There is no firmware yet, so the pin mapping still has to be tried out. The IRS2104 has built-in dead time, but we still need to check it against the real MOSFETs.

The board is four layers, 160 x 120 mm. In1 is a solid ground plane, there is a ground pour on the bottom layer, and a 10 mm wide battery bus pour on In2 joins the high-side drains. The drain pads have thermal via banks. The drains are live copper, so the heatsink needs an insulating pad.

## Current sensing

Each motor has two 10 mOhm shunts in parallel (5 mOhm) into an INA180A1 (gain 20), so 0.100 V per amp. The comparator reference is 2.477 V, which trips at about 24.8 A per channel, or about 49.5 A with both channels running. Each shunt dissipates about 1.5 W at the trip point. Tolerances and offsets are not included in these numbers.

The 20 A fuse protects against faults. It does not limit the running current. We still need a battery discharge limit, a charge/regen limit, and phase current limits once we know the motor ratings. The INA180 is unidirectional, so it cannot read regen current.

## Bus capacitor and regen

The 9 mF bank holds about 8.4 J at 43.2 V. With an external 100 Ohm precharge resistor it would start at 0.43 A (18.7 W) and take about 2.7 s to reach 95%. There is no precharge circuit on the board, so the pack side has to handle it.

The dump chopper (182k / 10k divider, 1M feedback) turns on at about 48 V and releases at about 47.4 V. That is above the 43.2 V full-charge voltage, so it does not protect individual cells. With the planned 10 Ohm dump resistor it would burn about 230 W at 48 V, so we need to work out how long it can run during a long downhill.

## Input protection

The LM74500-Q1 ideal diode is rated for 60 V recommended and 65 V absolute maximum on its source pin. The SMCJ45A TVS clamps at 72.7 V at 20.6 A, which is above that limit, so the transient protection still needs another look.

## Still to do

Motor ratings (KV, poles, resistance, phase current), thermal check on the MOSFETs and shunts, firmware, current limits, precharge, regen handling, and fitting the heatsink and terminals.
