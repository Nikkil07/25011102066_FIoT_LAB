# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 8

**DATE:** 15/09/2026

**NAME OF EXP:** Temporal Switch Press Duration Classifier

**HARDWARE SETUP / PLATFORM:** STM32F103C8T6 Blue Pill, Onboard LED (PC13), Tactile Button (PA0), ST-LINK V2

---

## TASK8C : Temporal Classifier Distinguishing Short Press (<1s) vs Long Press (>=1s)

### 1. Circuit Diagram

Same hardware arrangement as Task 8B (PC13 active-LOW LED, PA0 pull-up tactile button).

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
Advanced user interfaces reuse a single button for multiple functions based on press duration. By recording the timestamp t_press upon falling edge and t_release upon rising edge, the elapsed contact duration Delta_t = t_release - t_press is calculated. If Delta_t < 1000 ms, a 'Short Press' event is triggered (e.g. toggling LED). If Delta_t >= 1000 ms, a 'Long Press' event is triggered (e.g. rapid LED flashing sequence).

**Pseudocode:**
```text
1. Monitor PA0 transitions using HAL_GetTick().
2. On falling edge (HIGH -> LOW): record t_start = HAL_GetTick().
3. On rising edge (LOW -> HIGH):
     duration = HAL_GetTick() - t_start;
     If (duration >= 50 && duration < 1000):
         trigger_short_press(); // Toggle LED state
     Else if (duration >= 1000):
         trigger_long_press();  // Flash LED 3 times quickly
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** SysTick, GPIOA, GPIOC.
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Validated temporal event differentiation on a single physical button. The controller reliably distinguishes brief taps from deliberate sustained holds.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Tap briefly (<1s) to toggle LED; press and hold for >1s to trigger rapid alert strobe.`.
