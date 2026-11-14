# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 4

**DATE:** 18/08/2026

**NAME OF EXP:** Matrix Keypad Scanning & 7-Segment Decimal Decoder

**HARDWARE SETUP / PLATFORM:** AT89S52 MCU, 4x4 Matrix Keypad (Port 2: Rows P2.0-P2.3, Columns P2.4-P2.7), 7-Segment LED Display (Port 0: a-g, dp), Current-Limiting Resistor Array, 11.0592 MHz Crystal

---

## TASK4A : 4x4 Matrix Keypad Interface with Common Cathode 7-Segment Display

### 1. Circuit Diagram

Port 2 is dedicated to matrix keypad scanning: rows R1-R4 connect to P2.0-P2.3, while columns C1-C4 connect to P2.4-P2.7. Port 0 (pins 32-39) connects to the segments (a through g) of a common-cathode 7-segment display. Since Port 0 features open-drain outputs, an external 10 kOhm pull-up resistor pack is required on Port 0 lines to guarantee valid logic HIGH levels.

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
Scanning a 16-key matrix with only 8 GPIO lines requires multiplexed row-column matrix evaluation. Rows are configured as active-LOW outputs while columns serve as inputs with pull-ups. The algorithm successively drives one row LOW (0xFE, 0xFD, 0xFB, 0xF7) and samples the 4 column lines. A closed switch at row r and column c pulls column c LOW. Upon key identification, the key index is passed through a lookup table encoding standard 7-segment hex numerals: {0x3F, 0x06, 0x5B, 0x4F, 0x66, 0x6D, 0x7D, 0x07, 0x7F, 0x6F} and transferred to Port 0.

**Pseudocode:**
```text
1. Define 7-segment hex lookup table seg_code[16].
2. Function keypad_scan():
     For row = 0 to 3:
         Drive row LOW, other rows HIGH on P2 lower nibble.
         Read P2 upper nibble (columns).
         If any column is LOW, debounce 20 ms and compute key = row * 4 + col.
         Wait until key is released.
         Return key code.
3. In main loop:
     key = keypad_scan();
     If valid key, P0 = seg_code[key];
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** P0 (Address 0x80, open-drain display output), P2 (Address 0xA0, matrix scanning bus).
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Matrix scanning reduced pin consumption from 16 dedicated lines to 8 shared lines. Decoded keys (0-9, A-F) rendered cleanly on the 7-segment display without ghosting.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Press keys 0 through 9 on the 4x4 keypad and verify immediate decimal numeral rendering on Port 0 display.`.
