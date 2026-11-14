# CAT5 – Hardware Timer and Interrupt-Based LED Control

## EXPERIMENT DETAILS

**Experiment:** Continuous Lab Evaluation – 5 (CAT5)

**Date:** 10/08/2026 (Week 4)

**Course:** 25CS2741 – Fundamentals of Internet of Things + Lab

**Name of Experiment:** Hardware Timer and Interrupt-Based Timing for LED Control

## HARDWARE SETUP / PLATFORM

- **Microcontroller:** AT89S52
- **Architecture:** 8051
- **Development Board:** AT89S52 Development Board
- **Crystal Frequency:** 11.0592 MHz
- **Timer Used:** Timer 0
- **Hardware Debugging Tool:** Oscilloscope
- **LED GPIO Pin:** To be verified

## TASK NO

**Task 1: Timer-Based LED Control**

1. Configure Timer 0 to generate a precise periodic interrupt.
2. Use the timer interrupt to create a software time base.
3. Toggle an LED every 1 second.
4. Verify the generated timing using an oscilloscope.

## TIMER CONFIGURATION AND CALCULATIONS

For a classic 12-clock 8051 architecture:

**Crystal Frequency:** 11.0592 MHz

**Machine Cycle:**

Tmachine = 12 / 11.0592 MHz ≈ 1.085 µs

**For a 1 ms interrupt:**

Required Timer Count:

N = 1 ms / 1.085 µs ≈ 922

**Timer Preload Value:**

65536 - 922 = 64614 = 0xFC66

**Timer Register Values:**

- TH0 = 0xFC
- TL0 = 0x66

## CORE LOGIC / PSEUDOCODE / CODE BASE

### Timer 0 Configuration

- Configure Timer 0 for periodic interrupt generation.
- Load the calculated preload value into TH0 and TL0.
- Enable Timer 0 interrupt.
- Enable global interrupts.
- Start Timer 0.

### Timer Interrupt Service Routine (ISR)

- Handle Timer 0 overflow interrupts.
- Reload the timer registers.
- Maintain a software counter to track elapsed time.
- Generate a timing event after the required number of interrupts.

### LED Control

- Configure the required GPIO pin as an output.
- Toggle the LED whenever the 1-second timing event occurs.
- Repeat the process continuously.
- Avoid software delay loops for timing.

## WORKING FLOW

Crystal Oscillator (11.0592 MHz)

↓

Machine Cycle

↓

Timer 0 Counter

↓

Timer Overflow

↓

Interrupt

↓

Software Time Base

↓

1-Second LED Control

## INFERENCE

The experiment focuses on configuring Timer 0 of the AT89S52 microcontroller to generate periodic hardware interrupts. These interrupts are used to establish a software time base for controlling LED toggling at 1-second intervals. The generated timing is to be verified using an oscilloscope. The implementation avoids software delay loops and demonstrates interrupt-based timing in an 8051 microcontroller.

## PROJECT FILES

- `week5.uvproj` – Keil project file.
- `week5_t1.c` – C source file for Task 1.
- `STARTUP.A51` – Startup assembly file.
- `Objects/` – Contains generated build files.
- `Objects/week5.hex` – Generated HEX file.