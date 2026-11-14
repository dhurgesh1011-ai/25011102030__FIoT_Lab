# CAT5B.2 — Variable-Frequency Square-Wave Generator

## Overview

This AT89S52 program toggles `P1.1` from a Timer 0 interrupt. A push-button selects one of five timer reload settings for the output square wave.

## Objectives

- Configure Timer 0 in 16-bit Mode 1.
- Generate a square wave by toggling a GPIO in the timer ISR.
- Change the selected frequency using a debounced button.

## Hardware Requirements

- AT89S52 microcontroller with an 11.0592 MHz clock assumed by the source timing calculations
- Push-button input on `P3.2`
- Signal output on `P1.1`
- Oscilloscope or equivalent instrument to observe the output (as listed in the existing assignment notes)

## Software Requirements

- Keil µVision with the C51 toolchain
- `TASK5B_2.uvproj` declares a 33 MHz project clock, while the reload table is calculated for 11.0592 MHz. Verify the actual clock before interpreting the selected frequencies.

## Project Structure

```text
CAT5B.2/
├── README.md
├── STARTUP.A51
├── TASK5B_2.uvproj
├── circuit.png
├── main.c
└── Objects/
    └── TASK5B_2.hex
```

- `main.c` contains the reload tables, timer ISR, and button handling.
- `TASK5B_2.uvproj` includes `main.c` and `STARTUP.A51`.
- `Objects/TASK5B_2.hex` is the existing build output.

## Circuit Diagram

![Circuit diagram](circuit.png)

## Implementation

The source stores five high/low timer reload pairs in `th_table` and `tl_table`. Timer 0's ISR reloads the timer for the current `freq_mode` and complements `P1.1`. The foreground loop debounces an active-low button on `P3.2`, advances the mode modulo five, and waits for button release. The source associates the modes with 100 Hz, 500 Hz, 1 kHz, 2 kHz, and 5 kHz at 11.0592 MHz.

## Working Principle

1. Initialize `P1.1`, the button input, and Timer 0.
2. Timer interrupts reload the selected half-period and toggle the output.
3. A debounced button press advances to the next reload-table entry.
4. The ISR uses the new entry for subsequent output transitions.

## Output / Results

The intended output is a nominal 50% duty-cycle square wave with five button-selectable frequency settings. Actual frequencies depend on the oscillator and interrupt overhead; no measurement screenshot is present.

## Conclusion

The experiment demonstrates timer-reload selection, interrupt-driven GPIO toggling, and button-controlled signal generation.
