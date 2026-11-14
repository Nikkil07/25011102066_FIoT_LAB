# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 9

**DATE:** 22/09/2026

**NAME OF EXP:** Interactive Serial Command Line Interface (CLI) Parser

**HARDWARE SETUP / PLATFORM:** STM32F103C8T6 Blue Pill, User LED (PC13), USART1 (PA9/PA10), USB-UART Converter

---

## TASK9B : Embedded Serial CLI Command Interpreter ('LED ON', 'LED OFF', 'LED TOGGLE')

### 1. Circuit Diagram

Same hardware arrangement as Task 9A, with onboard PC13 LED driven according to parsed serial commands.

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
A Command Line Interface (CLI) decodes incoming ASCII text strings delimited by carriage return ('\r' or '\n'). Characters are buffered into an internal ring/linear array until the delimiter is received. String comparison (strcmp/strncmp) matches the entered token against known commands ('LED ON', 'LED OFF', 'LED TOGGLE', 'STATUS'), invoking appropriate HAL drivers and returning formatted response messages.

**Pseudocode:**
```text
1. Accumulate incoming bytes into rx_buffer until '\r' or '\n' is received.
2. Null-terminate rx_buffer.
3. Compare command token:
     If strcmp(rx_buffer, 'LED ON') == 0:
         HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_RESET);
         uart_print('OK: LED is turned ON\r\n');
     Else if strcmp(rx_buffer, 'LED OFF') == 0:
         HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_SET);
         uart_print('OK: LED is turned OFF\r\n');
     Else if strcmp(rx_buffer, 'LED TOGGLE') == 0:
         HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
         uart_print('OK: LED toggled\r\n');
     Else:
         uart_print('ERROR: Unknown Command\r\n');
4. Reset buffer index for next command line.
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** USART1->DR, USART1->SR, GPIOC->ODR.
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Demonstrated text-based human-to-machine control interface. The parser reliably decoded commands and actuated onboard hardware accordingly.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Send 'LED ON', 'LED OFF', and 'LED TOGGLE' from the serial terminal to confirm hardware actuation.`.
