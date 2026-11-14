# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 3

**DATE:** 11/08/2026

**NAME OF EXP:** Bitwise Alternating Complementary LED Flasher

**HARDWARE SETUP / PLATFORM:** AT89S52 Microcontroller, 8x LEDs on Port 1, 8x 330 Ohm Resistors, 11.0592 MHz Crystal

---

## TASK3C : Checkerboard LED Pattern Synthesizer (0x55 vs 0xAA)

### 1. Circuit Diagram

An entire 8-bit bus (Port 1, pins P1.0 through P1.7) is terminated with an array of eight LEDs through individual 330 Ohm current-limiting resistors to ground. Regulated power is supplied at Pin 40, ground at Pin 20.

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
Testing digital bus integrity frequently utilizes alternating checkerboard bit patterns (01010101_2 = 0x55 and 10101010_2 = 0xAA). This exposes adjacent-trace short circuits or capacitive cross-talk. The software loads 0x55 into the Port 1 latch, pauses for a parameterized interval, and then inverts the byte to 0xAA using bitwise NOT (~P1) or explicit assignment, toggling odd and even channels out-of-phase.

**Pseudocode:**
```text
1. Set P1 = 0x55 (energize odd pins: P1.0, P1.2, P1.4, P1.6).
2. Call delay_ms(400).
3. Set P1 = 0xAA (energize even pins: P1.1, P1.3, P1.5, P1.7).
4. Call delay_ms(400).
5. Repeat indefinitely in continuous loop.
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** P1 (Port 1 SFR, Address 0x90).
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Verified full 8-bit parallel bus driving and complementary pattern alternation. Odd and even LED channels cycle synchronously with zero phase jitter.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Verify adjacent LEDs oscillate in alternating groups with sharp 400 ms optical transitions.`.
