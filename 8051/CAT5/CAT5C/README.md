# CAT5C — Stepper Motor Speed and Direction Control

## Overview

This AT89S52 program sequences four output phases for a stepper motor. Timer 0 sets the step interval; separate push-buttons select speed and direction.

## Objectives

- Generate a repeating four-phase sequence using a timer interrupt.
- Select among three step intervals.
- Reverse the sequence direction using a push-button.

## Hardware Requirements

- AT89S52 microcontroller; source timing assumes an 11.0592 MHz clock
- Stepper motor and the interface shown in the schematic
- Speed button on `P3.2`
- Direction button on `P3.3`

The source drives the upper nibble of `P1`; it does not identify a motor driver or motor model. Confirm the interface in the circuit before connecting hardware.

## Software Requirements

- Keil µVision with the C51 toolchain
- `TASK5C.uvproj` declares a 33 MHz project clock, while the timer preload and speed intervals are based on 11.0592 MHz. Verify the actual clock before relying on step timing.

## Project Structure

```text
CAT5C/
├── README.md
├── STARTUP.A51
├── TASK5C.uvproj
├── main.c
├── CAT5C Stepper Motor Control Schematic.png
└── Objects/
    ├── README.md
    └── TASK5C.hex
```

- `main.c` contains timer, phase-sequence, speed, and direction logic.
- `TASK5C.uvproj` includes `main.c` and `STARTUP.A51`.
- `Objects/TASK5C.hex` is the existing build output; `Objects/README.md` is present but empty.

## Circuit Diagram

![Stepper motor control schematic](CAT5C%20Stepper%20Motor%20Control%20Schematic.png)

## Implementation

Timer 0 is configured in Mode 1 and reloaded with `0xFC66`, intended as a 1 ms tick at 11.0592 MHz. The ISR counts milliseconds until the selected interval (100, 50, or 10 ms) expires, then advances or reverses `step_idx` and writes one of `0x80`, `0x40`, `0x20`, or `0x10` to `P1.7`–`P1.4`. The foreground loop debounces `P3.2` to cycle speed and `P3.3` to toggle direction.

## Working Principle

1. Initialize the outputs, button inputs, and Timer 0.
2. Count timer interrupts until the selected step interval has elapsed.
3. Output the next phase in the forward or reverse sequence.
4. Update speed or direction when the corresponding button is pressed.
5. Continue generating steps.

## Output / Results

The intended result is a four-phase output sequence with three selectable step intervals and reversible phase order. Mechanical rotation and timing depend on the motor interface, actual clock, and hardware; no output screenshot is present.

## Conclusion

The experiment demonstrates timer-controlled step sequencing, speed selection, and direction reversal for a stepper-motor interface.
