# Engineering Projects Portfolio

![Verilog](https://img.shields.io/badge/HDL-Verilog-blue?style=for-the-badge&logo=verilog)
![Xilinx Vivado](https://img.shields.io/badge/EDA-Xilinx_Vivado-red?style=for-the-badge)
![FPGA](https://img.shields.io/badge/Hardware-Artix--7_FPGA-orange?style=for-the-badge)
![NodeMCU](https://img.shields.io/badge/Embedded-NodeMCU_ESP8266-green?style=for-the-badge&logo=espressif)

This portfolio contains complete design files, Verilog source code, architectural documentation, schematics, and simulation reports for three core hardware and embedded systems projects.

---

## 📂 Project Matrix

| Project | Domain | Architecture / Target | Core Technologies |
| :--- | :--- | :--- | :--- |
| **[Crypto Processor](./Project-1-Crypto-Processor)** | Digital Design & Security | Xilinx Artix-7 FPGA | Verilog HDL, AES-256, RSA-4096 |
| **[64-bit RISC-V Core](./Project-2-64-bit-RISC-V)** | Computer Architecture | RV64I Base Integer ISA | Verilog HDL, Vivado, GTKWave |
| **[Drowsiness Detection System](./Project-3-Drowsiness-Accident-Prevention)** | IoT & Embedded Systems | NodeMCU ESP8266 | C++, I2C LCD, IR Eye Sensors, Relays |

---

## 📌 Projects Detailed Overview

### 1. [Crypto Processor for Visual Data Security](./Project-1-Crypto-Processor)
* **Tech Stack:** Verilog HDL, Xilinx Vivado, Xilinx Artix-7 FPGA
* **Description:** Designed a hybrid cryptographic processor implementing **AES-256** for real-time visual pixel stream encryption and **RSA-4096** for secure asymmetric key exchange. Tested and verified via Xilinx Vivado behavioral simulations.

### 2. [64-bit RISC-V Processor Core](./Project-2-64-bit-RISC-V)
* **Tech Stack:** Verilog HDL, Xilinx Vivado, GTKWave / ModelSim
* **Description:** Implemented functional units for a **64-bit RISC-V (RV64I)** integer processor. Features a 64-bit ALU, 32x64-bit dual-port Register File (with `x0` protection), and a Data Memory interposer supporting byte, word, and doubleword sign-extended operations.

### 3. [Drowsiness Detection & Accident Prevention System](./Project-3-Drowsiness-Accident-Prevention)
* **Tech Stack:** NodeMCU ESP8266, C++, I2C, Embedded Systems
* **Description:** Built a real-time driver vigilance system using IR sensor eyeglasses to detect prolonged eye closure. Features automated piezo buzzer alerts, I2C status feedback, and relay-driven motor shutdown logic.

---

## 🛠️ Technical Competencies
* **Hardware Description Languages:** Verilog HDL (IEEE 1364-2005)
* **Computer Architecture:** RISC-V (RV64I ISA), ALU Design, Register Files, Memory Mapping
* **Embedded Hardware:** NodeMCU ESP8266, Microcontroller Interfacing, Relays, Sensors
* **Protocols & Standards:** I2C, SPI, UART
* **EDA Tools:** Xilinx Vivado Design Suite, Arduino IDE, ModelSim, GTKWave
