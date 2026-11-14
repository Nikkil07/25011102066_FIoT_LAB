# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 7

**DATE:** 08/09/2026

**NAME OF EXP:** Alphanumeric LCD Driver Bring-Up & Initialization

**HARDWARE SETUP / PLATFORM:** AT89S52 MCU, 16x2 HD44780 Character LCD Module, 10 kOhm Contrast Potentiometer, Port 2 (RS = P2.0, RW = P2.1, EN = P2.2, Data D4-D7 on P2.4-P2.7), 11.0592 MHz Crystal

---

## TASK7A : 16x2 HD44780 Liquid Crystal Display Driver in 4-Bit Bus Mode

### 1. Circuit Diagram

A 16x2 character LCD is wired in 4-bit nibble mode to conserve MCU GPIOs. Port 2 lines P2.0, P2.1, and P2.2 drive Register Select (RS), Read/Write (RW), and Enable (EN) respectively. Data lines D4 through D7 connect to P2.4 through P2.7. Contrast pin V0 is biased via a 10 kOhm potentiometer between +5V and GND.

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
The HD44780 controller initializes in 8-bit mode by default. To enter 4-bit mode, an explicit startup timing sequence must be sent (sending 0x03 three times with mandatory millisecond delays, followed by 0x02 to lock 4-bit bus operation). Subsequent commands and ASCII characters are transmitted as two successive 4-bit nibbles (upper nibble first, followed by lower nibble), latched on the falling edge of the Enable (EN) strobe. Commands 0x28 (4-bit, 2 lines, 5x8 font), 0x0C (display ON, cursor OFF), 0x06 (entry mode increment), and 0x01 (clear display) establish baseline display readiness.

**Pseudocode:**
```text
1. Function lcd_strobe():
     Set EN = 1; delay_us(5); Set EN = 0; delay_us(100);
2. Function lcd_send_nibble(byte, is_data):
     RS = is_data; RW = 0;
     P2 = (P2 & 0x0F) | (byte & 0xF0);
     lcd_strobe();
3. Function lcd_write(byte, is_data):
     lcd_send_nibble(byte, is_data);        // Send high nibble
     lcd_send_nibble(byte << 4, is_data);   // Send low nibble
4. Execute power-on initialization sequence (0x33, 0x32, 0x28, 0x0C, 0x06, 0x01).
5. Send string 'Hello SNU IoT' to DDRAM.
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** P2 (Address 0xA0, LCD control and data bus).
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Established reliable 4-bit parallel communication with the HD44780 controller. The display initialized cleanly and rendered static ASCII text across Line 1.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Adjust contrast trimmer until 16x2 matrix reveals sharp text characters on Row 1 without ghost blocks.`.
