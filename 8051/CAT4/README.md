# CAT4 — Matrix Keypad, Seven-Segment Display, and LED Control

## Overview

This AT89S52 program scans a matrix keypad, displays keys 0–9 on a seven-segment display, and uses the selected key to trigger LED patterns or direct LED states.

## Objectives

- Scan a keypad using Port 2.
- Display decimal digits using a seven-segment lookup table.
- Control LED patterns through key selection.

## Hardware Requirements

- AT89S52 development board
- Matrix keypad
- D168K seven-segment display (identified in the original assignment notes)
- LEDs
- USB programmer, as listed in the original assignment notes

The source assigns keypad scanning to Port 2, the segment pattern to Port 0, and LED patterns to Port 1. The exact display type and electrical connections are not established by the source.

## Software Requirements

- Keil µVision with the C51 toolchain
- The Keil project targets the AT89S52 and declares a 33 MHz project clock. The software delay duration depends on the actual clock and compiler.

## Project Structure

```text
CAT4/
├── README.md
├── STARTUP.A51
├── circuit.png
├── week4.uvproj
├── week4_logic.c
└── Objects/
    ├── README.md
    └── week4.hex
```

- `week4_logic.c` implements keypad scanning, digit output, LED modes, and delays.
- `week4.uvproj` includes `week4_logic.c` and `STARTUP.A51`.
- `Objects/week4.hex` is the existing build output; `Objects/README.md` is present.

## Circuit Diagram

![Circuit diagram](circuit.png)

## Implementation

`keypad_scan()` drives four row patterns on `P2.7`–`P2.4` and checks `P2.3`–`P2.1` for the first three rows; the fourth row checks `P2.2` for key 0. The implemented key map is 1–9 and 0, not all sixteen keys of a 4×4 keypad. `display_digit()` writes a 0–9 lookup-table value to `P0`. Keys 1–4 select individual `P1.1`–`P1.4` bits; keys 5–8 run LED patterns; key 9 clears the LEDs; key 0 clears the LEDs and displays 0.

## Working Principle

1. Initialize `P0`, `P1`, and the keypad port.
2. Repeatedly scan the keypad and detect a newly pressed key.
3. Display digits 0–9 on the seven-segment output.
4. Run the corresponding LED action for the key.
5. Continue scanning; pattern routines check for a different key so an action can be interrupted.

## Output / Results

The intended result is a displayed digit for each recognized key and the associated LED action. Keys beyond 0–9 are not handled by this source. No output screenshot is present.

## Conclusion

The program demonstrates keypad scanning, seven-segment digit encoding, and key-selected LED control using the AT89S52.
