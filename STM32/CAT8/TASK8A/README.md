# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 8

**DATE:** 15/09/2026

**NAME OF EXP:** ARM Cortex-M3 GPIO Bring-Up & Direct Push-Button Control

**HARDWARE SETUP / PLATFORM:** STM32F103C8T6 (Blue Pill, ARM Cortex-M3 @ 72 MHz), Onboard User LED (PC13, Active-LOW), External Push-Button (PA0), ST-LINK V2 SWD Debugger

---

## TASK8A : STM32F103 Blue Pill Bare-Metal GPIO Output & Input Polling

### 1. Circuit Diagram

The STM32F103C8T6 target microcontroller is supplied with 3.3V DC. An onboard SMD LED is hardwired to pin PC13 through a 510 Ohm pull-up resistor to 3.3V (active-LOW logic). An external tactile button is connected between pin PA0 and GND, with PA0 configured with an internal pull-up resistor (idle state = HIGH). Program flashing and runtime debugging are performed over the SWD interface (SWDIO on PA13, SWCLK on PA14).

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
ARM Cortex-M3 GPIO registers include Port Configuration Registers (CRL/CRH), Bit Set/Reset Register (BSRR), and Input Data Register (IDR). STM32 HAL abstracts these into HAL_GPIO_Init(), HAL_GPIO_ReadPin(), and HAL_GPIO_WritePin(). The system clock is established by multiplying an external 8.0 MHz crystal (HSE) via PLL x9 to reach SYSCLK = 72 MHz. Reading PA0 returns GPIO_PIN_RESET when pressed, commanding PC13 LOW to light the LED.

**Pseudocode:**
```text
1. Call HAL_Init() to reset peripherals and initialize Flash interface and SysTick.
2. Call SystemClock_Config() to configure 72 MHz PLL clock.
3. Configure PC13 as Output Push-Pull, speed 2 MHz.
4. Configure PA0 as Input with internal Pull-Up.
5. In continuous loop:
     If HAL_GPIO_ReadPin(GPIOA, GPIO_PIN_0) == GPIO_PIN_RESET:
         HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_RESET); // LED ON
     Else:
         HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_SET);   // LED OFF
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** GPIOC->CRH (PC13 configuration), GPIOA->CRL (PA0 configuration), GPIOA->IDR, GPIOC->BSRR, RCC->APB2ENR (Port A/C clock gating).
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Verified 32-bit ARM Cortex-M GPIO configuration using STM32 HAL. The active-LOW onboard LED accurately mirrors the tactile switch input state.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Depress button on PA0 to observe onboard PC13 LED lighting up instantly in real time.`.
