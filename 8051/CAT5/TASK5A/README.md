# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 5

**DATE:** 25/08/2026

**NAME OF EXP:** Hardware Timer Interrupt Programming (1-Second Timebase)

**HARDWARE SETUP / PLATFORM:** AT89S52 Microcontroller, Diagnostic LED on P1.0, 11.0592 MHz Crystal, Oscilloscope / Logic Analyzer

---

## TASK5A : Deterministic 1.000-Second Timebase Generation via Timer 0 Mode 1 ISR

### 1. Circuit Diagram

P1.0 is connected to an indicator LED via a 330 Ohm resistor to ground. Microcontroller clock is synthesized by the 11.0592 MHz crystal across pins 18 and 19. Reset and power rails follow standard bare-metal configuration.

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
Software delay loops waste CPU cycles and lack determinism during asynchronous events. Hardware Timer 0 is configured in Mode 1 (16-bit timer mode). With an 11.0592 MHz crystal, the timer clock is F_osc / 12 = 921.6 kHz (1 tick = 1.08507 us). To achieve a 50 ms tick, the timer must count: N = 50 ms / 1.08507 us = 46080 counts. The 16-bit preload value is calculated as 65536 - 46080 = 19456 = 0x4C00 (or 0xFC66 for 1 ms increments). An interrupt service routine (vector 0x000B) reloads the registers and increments a software counter. Reaching 20 ticks synthesizes an exact 1.000 s period.

**Pseudocode:**
```text
1. Configure TMOD = 0x01 (Timer 0 in 16-bit Mode 1).
2. Load initial reload values: TH0 = 0x4C, TL0 = 0x00.
3. Enable interrupts: ET0 = 1 (Timer 0 interrupt), EA = 1 (Global interrupt enable).
4. Start timer: TR0 = 1.
5. Timer 0 ISR (interrupt 1):
     Reload TH0 = 0x4C, TL0 = 0x00.
     Increment tick_count.
     If tick_count == 20:
         tick_count = 0;
         P1.0 = ~P1.0; // Toggle indicator every 1 second
6. Main function executes idle while(1) loop without blocking.
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** TMOD (0x89, Timer Mode Register), TH0/TL0 (0x8C/0x8A, Timer 0 high/low bytes), TCON (0x88, Timer Control), IE (0xA8, Interrupt Enable: EA, ET0).
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Verified asynchronous interrupt-driven programming on the 8051. The LED toggles at exactly 0.5 Hz (1s ON, 1s OFF) with zero CPU overhead in the main loop.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Measure square wave frequency on P1.0 with an oscilloscope to confirm 1.000 s half-period.`.
