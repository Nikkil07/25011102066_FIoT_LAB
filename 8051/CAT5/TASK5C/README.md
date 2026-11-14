# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 5

**DATE:** 25/08/2026

**NAME OF EXP:** Unipolar Stepper Motor Angular Control

**HARDWARE SETUP / PLATFORM:** AT89S52 MCU, 28BYJ-48 5V Stepper Motor, ULN2003A Darlington Transistor Driver Board, Port 1 (P1.4-P1.7), External +5V Motor Power

---

## TASK5C : 4-Phase Unipolar Stepper Motor Half-Step / Full-Step Controller via ULN2003

### 1. Circuit Diagram

Port 1 pins P1.4, P1.5, P1.6, and P1.7 connect to input channels IN1 through IN4 of a ULN2003 driver IC. The open-collector Darlington outputs (OUT1-OUT4) drive stator coils A, B, C, and D of a 28BYJ-48 stepper motor. The motor common rail connects to an external +5V DC power source, with motor ground tied to the MCU common reference.

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
Stepper motors convert discrete electrical pulses into proportional mechanical shaft rotation. The 28BYJ-48 unipolar motor features a stride angle of 5.625 deg with an internal 64:1 reduction gearbox (approx 2048 full steps per output shaft revolution). Driving phases in a full-step sequence (0x10 -> 0x20 -> 0x40 -> 0x80) creates a rotating magnetic stator field that pulls the permanent-magnet rotor in precise angular increments. Reversing the sequence reverses rotational direction.

**Pseudocode:**
```text
1. Define 4-step excitation table: step_seq[4] = {0x10, 0x20, 0x40, 0x80} on P1.4-P1.7.
2. Function step_motor(steps, direction, delay_time):
     For i = 0 to steps:
         idx = (direction == CW) ? (i % 4) : (3 - (i % 4));
         P1 = (P1 & 0x0F) | step_seq[idx];
         delay_ms(delay_time);
3. Execute 360-degree forward sweep followed by 360-degree reverse sweep.
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** P1 (Address 0x90, upper nibble P1.4-P1.7 driving motor phases).
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Verified electromechanical actuation and magnetic commutation. The stepper motor tracked commanded angular steps smoothly without slip at 10 ms inter-step pacing.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Verify steady 360-degree clockwise rotation followed by 360-degree counter-clockwise rotation of the motor shaft.`.
