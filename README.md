# 🌡️ Heterogeneous Monitoring System: Nios V, HPS, and FreeRTOS

## Project Overview

This project showcases the power of **System-on-Chip (SoC)** by utilizing the heterogeneous architecture of the **Altera Cyclone V SoC (Terasic SoCKit)**.

The main objective is to create a real-time temperature monitoring system where data acquisition and pre-processing are handled by real-time *soft-cores* in the FPGA fabric, and high-level communication is managed by the *hard-core* processor. Data is transmitted via Ethernet to a PC and visualized in real-time using **MATLAB**.

### 🎯 Key Objectives

* **Heterogeneous Processing:** Utilize the **Nios V** (RISC-V) *Soft Processor* and the **HPS** (ARM Cortex-A9) *Hard Processor*.
* **Real-Time Capability:** Employ **FreeRTOS** on the Nios V to ensure deterministic data control and acquisition.
* **Hardware Acceleration:** Implement a **Moving Average Filter** in the FPGA logic (Custom IP) for high-speed data pre-processing.
* **Integrated Communication:** Establish a communication channel using **Shared Memory (HPS DDR)** between the Nios V and the HPS.
* **Visualization:** Transmit data via Ethernet/Sockets (HPS/Linux) for real-time *streaming* visualization in **MATLAB**.

## 💻 System Architecture (Hardware & Software)

The project is divided into three main domains:

| Domain | Key Components | Function |
| :--- | :--- | :--- |
| **FPGA Fabric** | Nios V (RISC-V) + FreeRTOS | Manages ADT7301 sensor reading via SPI and interacts with the Hardware Accelerator. |
| **Custom IPs** | SPI Master, Moving Average Filter (AMM Slave) | VHDL/Verilog IPs for sensor interfacing and data pre-processing. |
| **HPS (ARM A9)** | Embedded Linux | Receives data packets from Shared Memory, manages the TCP/IP stack, and transmits data to the PC. |

### Data Flow

1.  **ADT7301** (Sensor) $\rightarrow$ **SPI Master IP** (FPGA)
2.  **Nios V/FreeRTOS** (Producer Task) reads raw data from the SPI IP.
3.  **Nios V** sends data to the **Accelerator (Filter)** in the FPGA and reads the filtered result back.
4.  **Nios V/FreeRTOS** (Consumer Task) writes the `Raw` and `Filtered` packet to the **Shared Memory (HPS DDR)**.
5.  **HPS/Linux** detects the signal (via Interrupt or Polling) and reads the data from memory.
6.  **HPS** sends the packets via **Ethernet (Sockets)** to the Host PC.
7.  **MATLAB** receives the data stream and updates the plot in real-time.

## 🛠️ Technologies Used

| Type | Tool / Technology | Version (Example) |
| :--- | :--- | :--- |
| **Hardware** | Terasic SoCKit (Cyclone V SEBA4) | - |
| **FPGA Tools** | Intel Quartus Prime Pro Edition / Standard | 23.1 |
| **Interconnection** | Platform Designer (Qsys) | - |
| **HPS OS** | Embedded Linux (or Bare-Metal) | Yocto / Linaro |
| **Soft Processor** | Nios V Soft Processor (RISC-V) | - |
| **RTOS** | FreeRTOS | V10.x |
| **Nios V IDE/Software**| RiscFree IDE | - |
| **Visualization** | MATLAB | R202x |

## 🚀 Directory Structure

The repository structure is organized as follows:

. ├── hardware/ │ ├── quartus/ # Quartus Project Files (.qpf, .qsf) │ ├── qsys/ # Platform Designer System (.qsys) │ └── custom_ip/ # VHDL/Verilog Code (SPI, Filter) ├── software/ │ ├── nios_v_freertos/ # Nios V C/FreeRTOS Application (RiscFree project) │ └── hps_linux/ # HPS C/C++ Application (socket reception and transmission) ├── tools/ │ └── matlab/ # MATLAB Scripts for data visualization └── README.md

## 📋 Prerequisites for Execution

To run this project, you will need:

1.  **Hardware:** Terasic SoCKit (Cyclone V SEBA4).
2.  **FPGA Software:** Intel Quartus Prime and Intel FPGA EDS (Embedded Development Suite).
3.  **Nios V Environment:** RiscFree IDE.
4.  **HPS Compiler:** Toolchain for ARMv7 (required for Linux or Bare-Metal on HPS).
5.  **Host PC:** MATLAB with an active license and Socket support (TCP/UDP).

## 💡 Next Development Steps

1.  Finalize the AMM interface for the **SPI Master IP** and the **Custom Accelerator IP**.
2.  Implement the VHDL/Verilog code for the **SPI Master** (ADT7301) and the **Moving Average Filter**.
3.  Develop the C driver (`adt7301_read_temp()`) in Nios V/FreeRTOS to interact with the SPI Master and Accelerator.
4.  Implement the Socket communication logic on the HPS.
5.  Create the MATLAB script for real-time data reception and plotting.

## 🤝 Contribution and License

Feel free to fork and contribute to this project, especially in optimizing the hardware filter or improving HPS-MATLAB communication.

This project is released under the **MIT License** (suggested).
