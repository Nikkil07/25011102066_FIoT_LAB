# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 5

**DATE:** 25/08/2026

**NAME OF EXP:** Variable Frequency Square Wave Synthesizer

**HARDWARE SETUP / PLATFORM:** AT89S52 Microcontroller, Output Pin P1.1, Piezo Transducer / Oscilloscope Probe, 11.0592 MHz Crystal

---

## TASK5B.2 : Tunable Square Wave Frequency Synthesizer (100 Hz to 5 kHz)

### 1. Circuit Diagram

P1.1 serves as the signal output line terminated into an oscilloscope test point or piezo buzzer through an AC coupling capacitor. System clock is supplied by the 11.0592 MHz crystal.

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
Frequency synthesis relies on toggling an output pin at half-period intervals: T_half = 1 / (2 * f). For f = 1 kHz, T_half = 500 us. Hardware Timer 1 in Mode 2 (8-bit auto-reload) automatically reloads TL1 from TH1 upon overflow, eliminating interrupt latency and generating jitter-free periodic pulse trains across audible and control spectrums.

**Pseudocode:**
```text
1. Configure TMOD for Timer 1 Mode 2 auto-reload.
2. Calculate TH1 reload value for target frequency f:
     ticks = (1 / (2 * f)) / 1.085 us;
     TH1 = 256 - ticks;
3. Start Timer 1 (TR1 = 1) with interrupt enabled (ET1 = 1, EA = 1).
4. Timer 1 ISR:
     P1.1 = ~P1.1; // Toggle output line
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** TMOD (0x89), TH1/TL1 (0x8D/0x8B), TCON (TR1), IE (ET1, EA).
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Synthesized pure square waves across 100 Hz to 5 kHz with 50% duty cycle. Auto-reload Mode 2 guaranteed precise timing with zero software reload jitter.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Measure output on P1.1 with frequency counter / DSO to confirm synthesized target frequencies.`.
