# Design and Implementation of Functional Blocks in a 64-Bit RISC-V Processor

## Project Overview
This project presents the design, implementation, and behavioral verification of key functional blocks for a 64-bit RISC-V processor based on the **RV64I Base Integer Instruction Set Architecture**. Designed using **Verilog HDL** and simulated in **Xilinx Vivado IDE**, this project implements the core execution and memory interfacing components suitable for a single-cycle or pipelined processor architecture.

---

## Architecture & Module Specification

### 1. Arithmetic and Logic Unit (ALU)
The ALU executes 64-bit arithmetic, bitwise logical, shift, and comparison operations required by RV64I execution and branch stages.

* **Port Description:**
  * `op_a` `[63:0]`: First 64-bit source operand (`rs1`).
  * `op_b` `[63:0]`: Second 64-bit source operand (`rs2` or immediate value).
  * `alu_control` `[3:0]`: 4-bit operation selection control signal.
  * `alu_result` `[63:0]`: 64-bit output result.
  * `zero` `[0:0]`: Flag raised high (`1'b1`) when `alu_result == 64'b0`.
  * `less_than` `[0:0]`: Flag raised for conditional branch operations (signed/unsigned comparison).

* **Supported Operations:**
  | Control Signal | Operation | Description |
  | :--- | :--- | :--- |
  | `4'b0000` | **ADD** | 64-bit Addition (`op_a + op_b`) |
  | `4'b0001` | **SUB** | 64-bit Subtraction (`op_a - op_b`) |
  | `4'b0010` | **AND** | Bitwise AND (`op_a & op_b`) |
  | `4'b0011` | **OR** | Bitwise OR (`op_a \| op_b`) |
  | `4'b0100` | **XOR** | Bitwise XOR (`op_a ^ op_b`) |
  | `4'b0101` | **SLL** | Shift Left Logical (`op_a << op_b[5:0]`) |
  | `4'b0110` | **SRL** | Shift Right Logical (`op_a >> op_b[5:0]`) |
  | `4'b0111` | **SRA** | Shift Right Arithmetic (`$signed(op_a) >>> op_b[5:0]`) |
  | `4'b1000` | **SLT** | Set Less Than Signed (`$signed(op_a) < $signed(op_b)`) |
  | `4'b1001` | **SLTU** | Set Less Than Unsigned (`op_a < op_b`) |

---

### 2. Register File Unit
A standard 32x64-bit general-purpose register file featuring dual asynchronous read ports and a single synchronous write port.

* **Features:**
  * **Registers:** 32 general-purpose registers (`x0` to `x31`), each 64 bits wide.
  * **Hardwired Zero (`x0`):** Register `x0` is permanently wired to `64'h0000000000000000`. Writes to `x0` are discarded.
  * **Asynchronous Read:** Outputs `rs1_read_data` and `rs2_read_data` immediately reflect changes in read address inputs (`rs1_addr`, `rs2_addr`).
  * **Synchronous Write:** Data (`reg_write_data`) is written to `rd_addr` on the rising edge of `clk` when write enable (`reg_write_en`) is active (`1'b1`).

* **Port Description:**
  * `clk`: Clock signal.
  * `reset`: Synchronous/Asynchronous active-high reset.
  * `rs1_addr` `[4:0]`, `rs2_addr` `[4:0]`: Read register selection addresses.
  * `rd_addr` `[4:0]`: Destination write register address.
  * `reg_write_data` `[63:0]`: Data payload to write into `rd_addr`.
  * `reg_write_en`: Global write control signal.
  * `rs1_read_data` `[63:0]`, `rs2_read_data` `[63:0]`: Asynchronous output read channels.

---

### 3. Data Memory & Interfacing Unit (MEM Stage)
Handles word, halfword, byte, and doubleword memory transfers while executing sign-extension or zero-extension for RV64I load/store instructions.

* **Supported Load/Store Instructions:**
  * **Doubleword (64-bit):** `LD` (Load Doubleword), `SD` (Store Doubleword)
  * **Word (32-bit):** `LW` (Load Word Sign-Extended), `LWU` (Load Word Zero-Extended), `SW` (Store Word)
  * **Halfword (16-bit):** `LH` (Load Halfword Sign-Extended), `LHU` (Load Halfword Zero-Extended), `SH` (Store Halfword)
  * **Byte (8-bit):** `LB` (Load Byte Sign-Extended), `LBU` (Load Byte Zero-Extended), `SB` (Store Byte)

* **Port Description:**
  * `clk`: System clock.
  * `mem_read`, `mem_write`: Control signals for memory access enablement.
  * `mem_op_type` `[3:0]`: Specifies byte-alignment and extension requirements (e.g., LB, LH, LW, LD, LBU, LHU, LWU).
  * `address` `[63:0]`: Byte-address calculated by the ALU.
  * `write_data` `[63:0]`: Data payload from `rs2` to store into memory.
  * `read_data` `[63:0]`: Properly extended 64-bit value retrieved from memory.

---

## Toolchain & Verification Setup

* **Hardware Description Language:** Verilog HDL (IEEE 1364-2005 standard)
* **EDA Tool:** Xilinx Vivado Design Suite
* **Simulation Type:** Behavioral Simulation (using Vivado Simulator / XSim)
* **Verification Process:** 
  * Self-checking testbenches were created for each individual module.
  * Corner-case testing executed for edge-case register writes (`x0` protection), sign-extension logic verification across varying byte offsets, and arithmetic overflow/underflow flag generation.

---

## Repository Structure
```text
├── Project-2-64-bit-RISC-V/
│   ├── 64-bit-RISC-V.pdf   # Complete Project Report
│   └── README.md                          # Documentation & Architecture Spec
