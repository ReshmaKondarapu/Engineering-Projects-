# Crypto Processor for Visual Data Security

[![FPGA Target](https://img.shields.io/badge/FPGA-Xilinx%20Artix--7-blue)](#hardware-specifications--synthesis-results)
[![HDL](https://img.shields.io/badge/Language-Verilog-orange)](#overview)
[![Tool](https://img.shields.io/badge/EDA-Xilinx%20Vivado-red)](#hardware-specifications--synthesis-results)

## Overview
This repository contains the Verilog HDL implementation and FPGA synthesis of a dedicated **Crypto Processor** designed to protect the integrity and confidentiality of visual data (pixel streams). 

The architecture employs a **hybrid cryptographic model**:
* **AES-256 (Symmetric Encryption):** Handles high-throughput, bulk encryption/decryption of 8-bit image pixel streams with low latency.
* **RSA-4096 (Asymmetric Encryption):** Manages key exchange by securely encrypting and decrypting the 256-bit secret key using public/private key pairs.

---

## System Architecture & State Machine

The processor operates through a fully integrated **Finite State Machine (FSM)** that manages key distribution and payload encryption sequentially:

* **IDLE:** Initializes registers, counters, and resets control flags upon system boot.
* **LOAD:** Serially streams in pixel data into an internal 216-bit payload buffer (27 pixels x 8 bits).
* **AES_ENC:** Pads and partitions raw data into 128-bit blocks, executing AES-256 encryption.
* **RSA_PROC:** Encrypts the 256-bit AES key using a 4096-bit RSA public key, then decrypts it using the private key to verify secure key transfer.
* **AES_DEC:** Decrypts the ciphered pixel blocks back to raw format using the validated AES key.
* **OUTPUT:** Serially outputs verified pixel data accompanied by data-valid handshaking flags.

---

## Hardware Specifications & Synthesis Results

Synthesized and validated on **Xilinx Vivado** targeting the **Artix-7 FPGA family**:

| Resource / Parameter | Measured Value | Utilization / Note |
| :--- | :--- | :--- |
| **Target Device** | Xilinx Artix-7 | FPGA Family |
| **Logic LUTs** | Minimal Overhead | ~1% Logic Utilization |
| **Flip-Flops (FF)** | Minimal Overhead | ~1% Register Utilization |
| **I/O Ports** | 50% Utilization | Accommodates high-width key/data buses |
| **Clocking (BUFG)** | 3% Utilization | Efficient clock routing |
| **Total Power Consumption** | ~7.004 W | Controlled power profile |
| **Thermal Junction Temp.** | 38.2 °C | Optimal thermal margin |

---

## Author & Academic Acknowledgments

* **Author:** Kondarpu Reshma  
* **Academic Guide:** Mr. Ch. Raghunatha Babu (Associate Professor)  
* **Department:** Electronics and Communication Engineering  
* **Institution:** Vignan's Lara Institute of Technology & Science (VLITS), Vadlamudi
