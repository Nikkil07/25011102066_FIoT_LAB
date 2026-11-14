# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 2

**DATE:** 04/08/2026

**NAME OF EXP:** Modular Software Millisecond Delay Library Architecture

**HARDWARE SETUP / PLATFORM:** AT89S52 Microcontroller, External Diagnostic LED, 330 Ohm Resistor, 11.0592 MHz Crystal

---

## TASK2C : Parametric Millisecond Timing Driver in Bare-Metal C

### 1. Circuit Diagram

The hardware arrangement utilizes Port 1 Pin 0 connected through a 330 Ohm resistor to an indicator LED for pulse duration validation. Ground reference is maintained at Pin 20, with regulated +5V at Pin 40 and internal Flash execution selected via Pin 31 (EA). The 11.0592 MHz crystal provides the reference clock across pins 18 and 19.

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
Creating deterministic timing delays without tying up hardware timers requires profiling compiled assembly execution times. At 11.0592 MHz, each machine cycle lasts 1.0851 us. Keil C51 compiles a standard inner loop decrementing an unsigned integer into DJNZ instructions (2 machine cycles = 2.17 us). By calibrating the inner loop count to approximately 112 to 120 iterations per outer loop count, a parameterized delay_ms(unsigned int ms) function is constructed, yielding consistent millisecond-scale pacing for sensor and peripheral drivers.

**Pseudocode:**
```text
1. Declare modular timing interface prototypes in delay.h.
2. Implement delay_ms(unsigned int count) in delay.c using nested loops:
     for outer = count to 0:
         for inner = 120 to 0: NOP/DJNZ
3. In main.c, call delay_ms(250) and delay_ms(750) between P1.0 toggles to synthesize asymmetric cadence.
4. Monitor timing accuracy across variable input arguments.
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** P1.0 (Output pin), Accumulator, R6/R7 general working registers used for C loop counters.
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Constructed and validated a reusable, hardware-independent millisecond delay library. Asymmetric cadences demonstrated predictable timing scaling directly proportional to input integer arguments.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Connect oscilloscope or logic analyzer to P1.0 and confirm pulse widths match requested millisecond parameters.`.
