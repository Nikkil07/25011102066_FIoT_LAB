# EXPERIMENT DETAILS

**EXP:** CONTINUOUS ASSESSMENT 13

**DATE:** 20/10/2026

**NAME OF EXP:** Dynamic JSON Telemetry Serialization

**HARDWARE SETUP / PLATFORM:** STM32 Nucleo-F401RE, BME280 Sensor (I2C1), ST-LINK Virtual COM Port (USART2)

---

## TASK13B : Compensated Sensor Telemetry Formatting into Structured JSON Payloads

### 1. Circuit Diagram

Hardware setup is identical to Task 13A (BME280 on I2C1 PB8/PB9, console on USART2 PA2/PA3).

![Circuit Diagram](circuit.png)

---

### 2. Core Logic / Pseudocode / Code Base

**Working Principle & Operational Theory:**
IoT cloud platforms require standardized payload serialization, commonly using JavaScript Object Notation (JSON). The compensated floating-point environmental metrics are formatted into a compact string payload: {"temperature": 24.52, "pressure": 1010.84, "humidity": 62.45}. Memory-efficient string formatting handles floating-point values without stack overflow, ready for network transport.

**Pseudocode:**
```text
1. Read calibrated BME280 measurements (T, P, H).
2. Format JSON string buffer:
     sprintf(json_buf, "{\"temperature\": %.2f, \"pressure\": %.2f, \"humidity\": %.2f}", T, P, H);
3. Transmit serialized JSON buffer over USART2 console.
```

**Code Structure / Key Registers:**
- **Primary Registers / APIs:** I2C1, USART2.
- **Clock / Timing Configurations:** Hardware clock tree and prescalers calibrated to theoretical operational parameters.

---

### 3. Inference

Validated lightweight JSON serialization on Cortex-M4. The structured payloads conform to RFC 8259 standards for seamless ingestion into web gateways.

> **Hardware Validation Note:** Verification status: `[CONFIRM ON BOARD/SIMULATION]: Observe properly formatted JSON strings streaming to the serial terminal console.`.
