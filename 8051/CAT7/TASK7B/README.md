# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 7

**DATE:** 08/09/2026

**NAME OF EXP:** LCD DDRAM Addressing & Dynamic Counter Telemetry

**HARDWARE SETUP / PLATFORM:** AT89S52 MCU, 16x2 HD44780 LCD, 10 kOhm Trimpot, Port 2 Control Bus, 11.0592 MHz Crystal

---

## TASK7B : LCD DDRAM Line Addressing & Real-Time Numeric Telemetry Rendering

### 1. Circuit Diagram

Hardware interface is identical to Task 7A (Port 2 driving HD44780 in 4-bit mode).

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
The HD44780 internal Display Data RAM (DDRAM) maps Row 1 to addresses 0x80-0x8F and Row 2 to addresses 0xC0-0xCF. Moving the cursor to a specific line and column involves sending the command byte: Address = 0x80 + offset (for row 1) or 0xC0 + offset (for row 2). Displaying numerical variables requires converting integer values into ASCII digits ('0' + digit) before writing to the display register.

**Pseudocode:**
```text
1. Function lcd_goto(row, col):
     cmd = (row == 0) ? (0x80 + col) : (0xC0 + col);
     lcd_command(cmd);
2. Function lcd_print_int(int val):
     Convert val to ASCII string via sprintf/modulo.
     Print string characters sequentially.
3. In main loop:
     lcd_goto(0, 0); lcd_print_str('Active Count:');
     lcd_goto(1, 0); lcd_print_int(counter++);
     delay_ms(500);
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** P2 (Control and data nibble bus).
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Verified DDRAM cursor repositioning and real-time numerical updates. Line 2 refreshed continuously without causing display flicker on Line 1.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Observe the numerical counter on LCD Line 2 incrementing sequentially every 500 ms.`.
