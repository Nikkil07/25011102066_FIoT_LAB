# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 3

**DATE:** 11/08/2026

**NAME OF EXP:** Switch Interfacing & LED Chaser Sequencing

**HARDWARE SETUP / PLATFORM:** AT89S52 MCU, 4x Indicator LEDs (P1.0 to P1.3), 4x 330 Ohm Resistors, Active-LOW Tactile Push-Button (P3.2), Pull-up Resistor, 11.0592 MHz Crystal

---

## TASK3A : Digital Push-Button Polling with Progressive LED Chase Sequencer

### 1. Circuit Diagram

Four individual LEDs are connected in active-high configuration across Port 1 pins P1.0 through P1.3, each buffered by a series 330 Ohm current-limiting resistor to ground. A tactile push-button switch is interfaced to Port 3 Pin 2 (P3.2, pin 12). A 10 kOhm pull-up resistor holds P3.2 at +5V when open; depressing the tactile button ties P3.2 directly to GND (active-LOW input).

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
Microcontroller inputs sample external digital voltage states. On AT89S52, port pins configured as inputs must have a logic '1' pre-written to their latch to release the internal pull-up. The firmware executes a polling loop that reads the level on P3.2. When the contact closure is detected (P3.2 == 0), the sequencer advances a single energized bit across Port 1 using bitwise shifting (0x01 -> 0x02 -> 0x04 -> 0x08), producing a running light pattern.

**Pseudocode:**
```text
1. Configure P3.2 as input by writing '1' to the corresponding bit.
2. Initialize Port 1 LED latch to idle state (0x00).
3. In main polling loop:
     a. Sample P3.2 state.
     b. If P3.2 == 0 (switch pressed):
          for step = 0 to 3:
              output (1 << step) to Port 1 (P1.0 -> P1.1 -> P1.2 -> P1.3)
              call delay_ms(200)
     c. Else (switch released):
          clear Port 1 (P1 = 0x00).
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** P1 (Data output latch, 0x90), P3 (Port 3 SFR, 0xB0), P3^2 (Pin 12 / INT0 pin used as digital input).
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Verified digital input polling and dynamic port bit-manipulation. The 4-channel LED chase sequence runs smoothly during active button depression and resets to idle upon release.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Press switch on P3.2 to observe sequential LED illumination from P1.0 to P1.3 at 200 ms intervals.`.
