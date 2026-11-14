# CAT3 — Button-Controlled LED Patterns

## Overview

The Keil project targets `cat_03.c`, which defines four LED patterns selected by a push-button. A second source file, `switch_logic.c`, contains a separate single-LED input example and is not included in the project.

## Objectives

- Control the low four bits of Port 1 as LED outputs.
- Read a push-button input and debounce it in software.
- Cycle through LED sequence, binary count, alternating pattern, and all-LED flash modes.

## Hardware Requirements

- AT89S52 microcontroller
- Four LEDs assigned to `P1.0`–`P1.3` in `cat_03.c`
- Active-low push-button assigned to `P2.6` in `cat_03.c`
- The circuit diagram is included; resistor and board connection details are not specified in the source.

## Software Requirements

- Keil µVision with the C51 toolchain
- The Keil project targets the AT89S52 and declares a 33 MHz project clock. The software delay loops have no verified timing calibration.

## Project Structure

```text
CAT3/
├── README.md
├── STARTUP.A51
├── Switch_logic_28.7.uvproj
├── cat_03.c
├── circuit.png
├── switch_logic.c
└── Objects/
    ├── README.md
    └── Switch_logic_28.hex
```

- `Switch_logic_28.7.uvproj` lists `cat_03.c`; `STARTUP.A51` is present in the folder but is not listed as a project source.
- `cat_03.c` contains the four-mode LED controller.
- `switch_logic.c` is a separate switch-to-single-LED example and is not listed in the project.
- `Objects/Switch_logic_28.hex` is the existing HEX file; `Objects/README.md` is present.

## Circuit Diagram

![Circuit diagram](circuit.png)

## Implementation

`cat_03.c` reads the active-low button on `P2.6`, waits briefly to debounce it, and waits for release before registering a press. Each press advances `state` through four modes. The modes output a walking one, a four-bit count, alternating `0x05`/`0x0A` patterns, or alternating all-off/all-on values on the low nibble of `P1`. `P2.3` is also assigned low during initialization, but its purpose is not explained by the source.

`switch_logic.c` separately reads `P2.0` and sets the `P1.0` LED according to that input; the source sets `P2.5` low. It has no project entry in the supplied Keil project.

## Working Principle

1. The project program initializes the button input and enters its main loop.
2. A debounced button press advances the pattern state, wrapping after the fourth mode.
3. The selected mode writes LED patterns to `P1.0`–`P1.3` with software delays.
4. The loop continues checking for a button press and updating the LEDs.

## Output / Results

The intended project output is the four LED patterns described above. The project source contains `delay_ms(50 0);` in the all-flash function, which is not a valid C integer argument; therefore, a successful build or observed output cannot be confirmed from the checked-in source. No output screenshot is present.

## Conclusion

The project is intended to demonstrate GPIO output patterns, active-low button input, and software debouncing. The invalid delay argument must be resolved in the source before the project can be built; source files were left unchanged.
