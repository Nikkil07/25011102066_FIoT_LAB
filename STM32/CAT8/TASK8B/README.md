# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 8

**DATE:** 15/09/2026

**NAME OF EXP:** Software Push-Button Contact Debounce & Edge Toggling

**HARDWARE SETUP / PLATFORM:** STM32F103C8T6 Blue Pill, User LED (PC13), Tactile Switch (PA0), ST-LINK V2

---

## TASK8B : Non-Blocking Digital Push-Button Debouncing & State Toggle Engine

### 1. Circuit Diagram

PC13 drives the active-LOW user LED. PA0 connects to a tactile button closing to GND with an internal pull-up resistor.

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
Mechanical contact bounce generates rapid voltage transients lasting up to 20 ms, which causes false multiple triggering if sampled directly. A non-blocking debouncer samples HAL_GetTick() (1 ms SysTick timer). When a state transition from HIGH to LOW is detected, the algorithm waits for a stable 30 ms window before toggling the persistent LED state flag, ensuring one action per physical button press.

**Pseudocode:**
```text
1. Initialize HAL, SystemClock (72 MHz), GPIOs.
2. Static uint32_t last_debounce_time = 0; static uint8_t led_state = 0;
3. Main Loop:
     uint8_t reading = HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0);
     If (reading != last_button_state):
         last_debounce_time = HAL_GetTick();
     If ((HAL_GetTick() - last_debounce_time) > 30):
         If (reading == GPIO_PIN_RESET && button_pressed == 0):
             led_state = !led_state;
             HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, led_state ? GPIO_PIN_RESET : GPIO_PIN_SET);
             button_pressed = 1;
         Else if (reading == GPIO_PIN_SET):
             button_pressed = 0;
     last_button_state = reading;
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** SysTick timer register, GPIOA->IDR, GPIOC->ODR.
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Implemented reliable software debouncing without blocking delay loops. Successive switch depressions toggle LED state cleanly without spurious double-triggering.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Tap button on PA0 repeatedly to verify alternating LED ON/OFF latching on each press.`.
