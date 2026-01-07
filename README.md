 JTAG 
 Introduction

Modern VLSI chips are extremely complex, making post-manufacturing testing difficult.
**Design for Testability (DFT)** techniques are used to ensure chips can be tested, debugged, and validated efficiently.

One of the most important DFT techniques is **JTAG (Boundary Scan)**.

This document explains **DFT and JTAG from scratch**, including:

* TAP controller
* Registers
* Instructions
* Operation flow
* Old vs modern JTAG
* Interview-oriented explanations

---

## What is Design for Testability (DFT)?

### Definition

**Design for Testability (DFT)** refers to design techniques that make it easier to test a chip after fabrication.

### Why DFT is needed?

* Manufacturing defects
* Process variations
* Limited physical access to internal nodes
* Increasing SoC complexity

### Common DFT Techniques

* Scan Chains
* Built-In Self-Test (BIST)
* Boundary Scan (**JTAG**)

**JTAG is mainly used for boundary scan, debugging, and programming.**

---

## 2️What is JTAG?

### Definition

**JTAG** stands for **Joint Test Action Group**.
It is standardized as **IEEE 1149.1**.

### Why JTAG was introduced?

* High pin density packages (BGA, QFN)
* No physical access to internal signals
* Board-level testing challenges

### Applications of JTAG

* Boundary scan testing
* Flash/FPGA programming
* CPU debugging
* In-system testing

---

## JTAG Signals (Pins)

| Signal          | Name             | Function                       |
| --------------- | ---------------- | ------------------------------ |
| TCK             | Test Clock       | Clocks TAP controller          |
| TMS             | Test Mode Select | Controls TAP state transitions |
| TDI             | Test Data In     | Serial input data              |
| TDO             | Test Data Out    | Serial output data             |
| TRST (optional) | Test Reset       | Resets TAP controller          |

---

##TAP Controller (Heart of JTAG)

![Image](https://www.xjtag.com/wp-content/uploads/tap_state_machine.gif)

![Image](https://media.licdn.com/dms/image/v2/D4D12AQEEILVO7TbWfg/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1712390697969?e=2147483647\&t=SiXzappJJSGqV7sLir6DKmDVlk5zJDux6RiPeM4Kvh0\&v=beta)

### What is TAP Controller?

TAP (Test Access Port) controller is a **Finite State Machine (FSM)** that controls all JTAG operations.

### Major TAP States

* Test-Logic-Reset
* Run-Test / Idle

### Data Register Path

* Select-DR-Scan
* Capture-DR
* Shift-DR
* Update-DR

### Instruction Register Path

* Select-IR-Scan
* Capture-IR
* Shift-IR
* Update-IR
 **TMS decides the next state, TCK clocks the FSM.**

---

## JTAG Registers

### 5.1 Instruction Register (IR)

* Selects which JTAG operation to perform
* Width depends on implementation (e.g., 4-bit, 5-bit)

---

### 5.2 Data Registers (DR)

#### Boundary Scan Register (BSR)

* Connected to I/O pins
* Used for boundary scan testing

#### Bypass Register

* **1-bit register**
* Minimizes delay when device is not targeted

#### IDCODE Register

* Stores:

  * Manufacturer ID
  * Part number
  * Version

---

##  JTAG Instructions

| Instruction | Purpose                        |
| ----------- | ------------------------------ |
| EXTEST      | External boundary scan testing |
| SAMPLE      | Sample I/O pin values          |
| INTEST      | Internal logic testing         |
| BYPASS      | Skip device in chain           |
| IDCODE      | Read device identification     |

### Typical Instruction Flow

1. Move TAP to **Shift-IR**
2. Load instruction
3. Update IR
4. Move to **Shift-DR**
5. Perform data operation

---

## JTAG Operation Flow (Step-by-Step)

```text
1. Reset TAP Controller
2. Load Instruction Register (IR)
3. Update IR
4. Access Data Register (DR)
5. Shift data in/out
6. Update DR
```

**All JTAG actions happen serially through TDI/TDO.**

---

##  Old JTAG vs Modern JTAG

### Classic JTAG (IEEE 1149.1)

* Boundary scan only
* Limited debug features
* Slower access
* Board-level testing focus

---

### Modern JTAG Extensions

| Standard          | Purpose                    |
| ----------------- | -------------------------- |
| IEEE 1149.7       | Compact JTAG (fewer pins)  |
| IEEE 1500         | Embedded core testing      |
| IEEE 1687 (IJTAG) | Internal instrument access |

**IJTAG allows access to internal sensors, monitors, and debug logic inside SoCs.**

---

## Simple JTAG Example

### Assumptions

* IR width = 4 bits
* Instruction = IDCODE

### Steps

1. Reset TAP
2. Go to Shift-IR
3. Shift `IDCODE` instruction
4. Update IR
5. Go to Shift-DR
6. Read device ID via TDO

---

##Interview-Focused Notes

### Q1: Why is BYPASS register only 1-bit?

✔ To reduce delay when the device is not selected in a JTAG chain.

### Q2: Difference between SAMPLE and EXTEST?

* **SAMPLE** → Observe pin values
* **EXTEST** → Drive pin values externally

### Q3: Why does IR width vary?

✔ Depends on number of supported instructions.

### Q4: Is JTAG synchronous or asynchronous?

✔ Synchronous (driven by TCK).


