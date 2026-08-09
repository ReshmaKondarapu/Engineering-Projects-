# Detection of Drowsiness and Accident Prevention System
![NodeMCU](https://img.shields.io/badge/Board-NodeMCU_ESP8266-blue?style=flat-square)
![Language](https://img.shields.io/badge/Language-C%2B%2B-orange?style=flat-square)
![Interface](https://img.shields.io/badge/Protocol-I2C-green?style=flat-square)
![IDE](https://img.shields.io/badge/IDE-Arduino_IDE-00979D?style=flat-square)

## Overview
An embedded safety system designed to monitor driver alertness in real time and automatically intervene to prevent fatigue-induced vehicle accidents. Built around the **NodeMCU ESP2866 / ESP8266** board, the system uses IR eye-blink sensor spectacles to detect prolonged eye closure. Upon detecting drowsiness, it triggers auditory/visual alerts (buzzer and LED) and controls a relay to disable the vehicle's DC motor driving mechanism.

---

## Technical Specifications & Hardware Modules

### 1. Processing Unit — NodeMCU ESP8266
* **CPU:** Tensilica 32-bit RISC Xtensa LX106[cite: 2]
* **Operating / Input Voltage:** 3.3V operating voltage, 7–12V external input range[cite: 2]
* **I/O Capabilities:** 16 Digital I/O pins (DIO) and 1 10-bit Analog Input pin (ADC0)[cite: 2]
* **Features:** Integrated Wi-Fi module, DIP-30 header configuration, programmable via Arduino IDE[cite: 2].

### 2. Sensor & Actuator Interface
* **Eye Blink Sensor Spectacles:** Eyeglass frame fitted with an IR transmitter LED and IR photodiode detector[cite: 2]. Measures reflected infrared light intensity off the eyelid to distinguish between normal blinks and prolonged closure[cite: 2].
* **Electromechanical Relay Module:** Acts as an automated power cut-off switch controlling high-power loads (DC motor) using low-voltage logic signals from the MCU[cite: 2].
* **Piezoelectric Buzzer:** Operates on the inverse piezoelectric effect within the 2–4 kHz frequency range to emit high-decibel auditory warnings[cite: 2].
* **DC Motor & Wheel Assembly:** Simulates the vehicle drivetrain powered by a dedicated 9V DC battery source[cite: 2].
* **Liquid Crystal Display (I2C LCD):** $16 \times 2$ character display driven via I2C interface (address `0x27`) to display real-time operational state ("NORMAL" vs. "DANGER")[cite: 2].
* **Status LEDs:** Visual indicators displaying real-time power and alert statuses[cite: 2].

---

## Circuit Schematic & Pin Mapping

### NodeMCU ESP8266 Pin Assignments
| NodeMCU Pin | Signal / Connected Component | Hardware Function / Interface |
| :--- | :--- | :--- |
| `D0` | `EYESENSOR` Output | Digital input from IR Eye Blink Sensor[cite: 2] |
| `D5` | `BUZZ` Output | Piezo Buzzer control channel[cite: 2] |
| `D6` | `LED` Output | Status Indicator LED channel[cite: 2] |
| `D7` | `RELAY` Output | Relay Module trigger signal[cite: 2] |
| `I2C` (`SDA`/`SCL`) | LCD Module (`0x27`) | Software I2C bus (`GPIO4`/`GPIO5`)[cite: 2] |
| `VIN` / `GND` | Power Lines | 9V DC Battery / System Common Ground[cite: 2] |

---

## System Operational Workflow

1. **System Initialization:** Power is supplied via a 9V battery source to the DC motor assembly and NodeMCU board[cite: 2]. The MCU initializes GPIO direction states, serial communication at 9600 baud, and the I2C LCD backlight[cite: 2].
2. **Nominal State ("NORMAL"):**
   * The IR sensor on the glasses detects open eyes (`Data == 1` / High logic)[cite: 2].
   * Relay remains energized (`RELAY = 1`), allowing power flow to keep the DC motor and vehicle wheel spinning[cite: 2].
   * The green status LED stays lit (`LED = 1`), the buzzer is silent (`BUZZ = 0`), and the LCD displays `"NORMAL"`[cite: 2].
3. **Hazard State ("DANGER"):**
   * If the driver feels drowsy and closes their eyes, the closed eyelid interrupts/reflects the IR signal, bringing sensor logic low (`Data == 0`)[cite: 2].
   * The MCU immediately de-energizes the relay (`RELAY = 0`), cutting power to the DC motor to slow down and stop the vehicle drivetrain[cite: 2].
   * The piezo buzzer sounds (`BUZZ = 1`), warning LEDs illuminate, and the LCD screen switches to display `"DANGER"`[cite: 2].

---

## Embedded Source Code

```cpp
#include <LiquidCrystal_I2C.h>

// Initialize LCD display at I2C address 0x27 for 16 chars and 2 lines
LiquidCrystal_I2C lcd(0x27, 16, 2);

const int BUZZ = D5;
const int LED = D6;
const int RELAY = D7;
const int EYESENSOR = D0;

int Data;

void setup() {
  lcd.init();
  lcd.backlight();
  
  pinMode(RELAY, OUTPUT);
  pinMode(BUZZ, OUTPUT);
  pinMode(LED, OUTPUT);
  pinMode(EYESENSOR, INPUT);
  
  Serial.begin(9600);
}

void loop() {
  Data = digitalRead(EYESENSOR);
  Serial.print("Sensor Logic: ");
  Serial.println(Data);
  
  if (Data == 0) {
    // Drowsiness detected: Trigger alarm and disable drive motor
    digitalWrite(LED, LOW);
    digitalWrite(RELAY, LOW);
    digitalWrite(BUZZ, HIGH);
    
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("DANGER");
    delay(1000);
  } 
  else {
    // Standard operation: Maintain normal driving state
    digitalWrite(LED, HIGH);
    digitalWrite(RELAY, HIGH);
    digitalWrite(BUZZ, LOW);
    
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("NORMAL");
    delay(1000);
  }
}
