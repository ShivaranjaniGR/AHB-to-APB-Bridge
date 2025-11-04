# AHB-to-APB-Bridge

The **AHB to APB Bridge** acts as an **AHB slave** and the **only APB master**, providing a link between the **high-speed AHB** and the **low-power APB**. Read and write transfers on the AHB are converted into equivalent transfers on the APB.

---

## About the AMBA Buses

The **Advanced Microcontroller Bus Architecture (AMBA)** defines a standard on-chip communication interface for high-performance embedded microcontrollers.  
It consists of three main buses:

- **Advanced High-performance Bus (AHB)**
- **Advanced System Bus (ASB)**
- **Advanced Peripheral Bus (APB)**

---

## Advanced High-performance Bus (AHB)

The **AMBA AHB** targets **high-performance, high-frequency** system modules. Acting as the main system backbone, AHB connects processors, on-chip memories, and external memory interfaces with low-power peripherals.  
It is optimized for performance and supports automated design flow, synthesis, and testing.

---

## Advanced System Bus (ASB)

The **AMBA ASB** provides a high-performance communication interface, suitable where the advanced features of AHB are unnecessary.  
It efficiently connects processors, on-chip memories, and external interfaces to low-power peripheral functions.

---

## Advanced Peripheral Bus (APB)

The **AMBA APB** is designed for **low-power peripherals**.  
It minimizes power consumption and interface complexity while maintaining compatibility with both AHB and ASB system buses.

---

### Overall Architecture

![image](https://github.com/Hashmi-Vardhan/AHB-to-APB-Bridge/assets/133083707/ea4d35cf-492e-4e59-9830-be47ba7c7a6f)

---

## Basic Terminology

**Bus Cycle**  
A bus cycle is defined as one complete clock period (rising-edge to rising-edge).

**Bus Transfer**  
- An **AHB** or **ASB** bus transfer is a read or write operation that may take multiple bus cycles and completes when the slave sends a response.  
- An **APB** transfer always requires **two bus cycles** (setup and enable phases).

**Burst Operation**  
A **burst** is a series of data transactions initiated by a master to consecutive addresses.  
Each transaction has a consistent width (byte, halfword, word).  
> Note: Burst operations are **not supported on the APB**.

---

## AMBA Signals

### AMBA AHB Signals
![image](https://github.com/Hashmi-Vardhan/AHB-to-APB-Bridge/assets/133083707/192df179-e8df-483f-b5be-70d25092bde1)

### AMBA APB Signals
![image](https://github.com/Hashmi-Vardhan/AHB-to-APB-Bridge/assets/133083707/f3a91900-65d6-4bfb-86f3-ad34e3f67cba)

---

## Implementation

### Objective

To **design and simulate** a synthesizable **AHB-to-APB bridge** interface using **Verilog HDL** and verify functionality through **single read** and **single write** test cases with AHB Master and APB Slave testbenches.

The bridge performs the following operations:
- Latches and holds address lines stable during transfers  
- Decodes addresses and asserts the appropriate **PSELx** line (one active at a time)  
- Drives write data onto the APB during write cycles  
- Reads APB data back onto the AHB bus during read cycles  
- Generates the timing strobe **PENABLE** for APB transfers  
- Successfully executes **single read and write operations**

**Bridge Interface Diagram:**

![image](https://github.com/Hashmi-Vardhan/AHB-to-APB-Bridge/assets/133083707/dc157751-d470-4681-b238-846ee2940766)

---

## Design Modules

### AHB Slave Interface
The **AHB slave** responds to transactions initiated by an AHB master.  
It uses the `HSELx` signal from the decoder to determine when to respond.  
Address and control signals are provided by the master.

### APB Controller
The **APB controller** (inside the bridge) consists of:
- A **state machine** that generates AHB and APB control signals  
- Address decoding logic that produces **PSELx** for the appropriate peripheral  

---

## Notes

- The repository includes **AHB Master** and **APB Slave** modules used as **testbenches** to drive and validate the bridge.  
- Only the **Bridge** module is **synthesizable**.  
- Screenshots from simulation and synthesis tools are provided below.

---

## Simulation Results

### SINGLE WRITE Operation
![image](https://github.com/Hashmi-Vardhan/AHB-to-APB-Bridge/assets/133083707/2e2d96a7-a1db-44ea-9938-7a4f913ff82a)

### SINGLE READ Operation
![image](https://github.com/Hashmi-Vardhan/AHB-to-APB-Bridge/assets/133083707/b1bb7367-965a-4bb7-8c01-5de09d3c87b8)

### BURST WRITE (INCR4)
![image](https://github.com/Hashmi-Vardhan/AHB-to-APB-Bridge/assets/133083707/81992a07-de71-4a0c-a9c5-6045ecd914a2)

### BURST READ (INCR4)
![image](https://github.com/Hashmi-Vardhan/AHB-to-APB-Bridge/assets/133083707/c198413a-b483-4689-83db-f7d5649d859c)

---


## Further Work

- Add burst read/write support in AHB Master  
- Implement arbitration logic and signals for multi-master configurations  

---

