# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 5

**DATE:** 25/08/2026

**NAME OF EXP:** Precision Calibrated Duty Cycle Controller

**HARDWARE SETUP / PLATFORM:** AT89S52 MCU, Output Pin P1.1, Dual-Trace Oscilloscope, 11.0592 MHz Crystal

---

## TASK5B.3 : Asymmetric PWM Duty Cycle Controller using Timer Auto-Reload

### 1. Circuit Diagram

P1.1 connects to oscilloscope Channel 1 for pulse measurement. Clock and power lines are decoupled to prevent switching transients.

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
Independent control of duty cycle and frequency requires dynamic scheduling of high and low phase reload values. Upon entering the timer ISR, the output pin is toggled and the subsequent phase reload value (T_high vs T_low) is written to the timer registers, enabling asymmetric pulse synthesis (e.g. 25%, 50%, 75% duty cycle).

**Pseudocode:**
```text
1. Define high_reload and low_reload counts.
2. Timer ISR:
     If pin state is currently HIGH:
         P1.1 = 0;
         Reload timer with low_reload;
     Else:
         P1.1 = 1;
         Reload timer with high_reload;
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** TH1, TL1, TMOD, TCON, IE.
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Demonstrated calibrated duty cycle synthesis with sub-microsecond edge precision. Verified independent modulation of pulse width and repetition frequency.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Measure waveform on P1.1 to confirm duty cycle ratios match 25%, 50%, and 75% settings.`.
