# CAT7C — Keypad-Controlled LCD Character Movement

## Overview

This AT89S52 program scans a 4×4 keypad and moves an asterisk around a 16×2 LCD using the 2, 8, 4, and 6 keys.

## Objectives

- Scan a matrix keypad using Port 1.
- Use LCD cursor positioning to move a displayed character.
- Restrict the character to the two-row, sixteen-column display area.

## Hardware Requirements

- AT89S52 microcontroller
- 4×4 matrix keypad: rows on `P1.0`–`P1.3`, columns on `P1.4`–`P1.7`
- HD44780-compatible 16×2 LCD: data lines on `P2.4`–`P2.7`, RS on `P2.0`, enable on `P2.1`

The source does not specify the external connection of the LCD read/write pin.

## Software Requirements

- Keil µVision with the C51 toolchain
- The project declares a 33 MHz clock. Keypad debounce and LCD delays are software loops whose timing depends on the actual clock and compiler.

## Project Structure

```text
CAT7C/
├── README.md
├── STARTUP.A51
├── TASK7C.uvproj
├── circuit.png
├── lcd.c
├── lcd.h
├── main.c
└── Objects/
    ├── README.md
    └── TASK7C.hex
```

- `main.c` scans the keypad and updates the asterisk position.
- `lcd.c` implements 4-bit LCD commands/data and cursor positioning.
- `lcd.h` declares the LCD functions.
- `TASK7C.uvproj` includes `lcd.c`, `main.c`, and `STARTUP.A51`.
- `Objects/TASK7C.hex` is the existing build output; `Objects/README.md` is present but empty.

## Circuit Diagram

![Circuit diagram](circuit.png)

## Implementation

`keypad_scan()` drives one active-low row at a time on `P1.0`–`P1.3`, checks the upper nibble for a column press, debounces it, and waits for release. `main()` initializes the LCD and places `*` at row 0, column 0. Key `2` moves up, `8` down, `4` left, and `6` right; the bounds checks keep the cursor within rows 0–1 and columns 0–15. Other keys are ignored.

## Working Principle

1. Initialize the keypad port and LCD; display `*` at the starting position.
2. Scan keypad rows and identify a pressed key.
3. For a directional key, erase the asterisk at its current position.
4. Update the row or column if the move remains within the LCD bounds.
5. Draw the asterisk at the new position and continue scanning.

## Output / Results

The intended output is a movable `*` controlled by keypad keys 2/8/4/6, constrained to the 16×2 display. No output screenshot is present.

## Conclusion

The experiment demonstrates keypad matrix scanning and coordinated LCD cursor control for interactive character movement.
