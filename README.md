# Safety-Critical High-Voltage Insulation Monitoring Device (IMD)

[![Embedded C](https://img.shields.io/badge/Language-C-blue.svg)]()
[![Hardware](https://img.shields.io/badge/MCU-STM32-red.svg)]()
[![Domain](https://img.shields.io/badge/Domain-Automotive_&_EV-success.svg)]()
[![Safety](https://img.shields.io/badge/Safety-High_Voltage-critical.svg)]()


## 📌 Project Overview
This repository contains the embedded software and system architecture for a **High-Voltage Insulation Monitoring Device (IMD)**, developed for an electric vehicle battery isolation project (Voltaris Team). 

In high-voltage DC systems, a loss of galvanic isolation between the HV bus and the vehicle chassis poses a lethal threat. This embedded system continuously monitors the insulation resistance of both the positive and negative busbars in real-time, instantly triggering a safety shutdown sequence if the resistance drops below standard human safety thresholds.

## 🧠 The Algorithm: 4-Step Unbalanced Bridge
Standard voltage dividers fail to accurately measure insulation degradation when faults occur on both busbars simultaneously. To overcome this, the system implements a complex **4-Step Unbalanced Bridge Algorithm**:

1.  **State 1 (Baseline):** All diagnostic relays are open. Baseline system voltage is measured to determine the current state of the DC bus.
2.  **State 2 (Positive Polling):** An optical relay connects a known precision resistor between the positive HV bus and the chassis ground. The microcontroller samples the resulting voltage drop via hardware ADCs.
3.  **State 3 (Negative Polling):** The positive relay opens, and the negative relay closes, connecting the known resistor to the negative bus. The ADC captures the new voltage state.
4.  **State 4 (Algebraic Resolution):** The STM32 processes the sampled analog values using a system of linear equations to dynamically calculate the exact insulation resistance of both $R_p$ (positive to ground) and $R_n$ (negative to ground), completely independent of battery voltage fluctuations.

## ⚙️ Hardware & Software Architecture
*   **Microcontroller:** STM32 Series (ARM Cortex-M architecture).
*   **Signal Conditioning:** High-voltage signals are scaled down via precision resistor networks and buffered through operational amplifiers before reaching the MCU.
*   **Switching Logic:** Optically isolated relays controlled via precisely timed GPIO sequences to prevent high-voltage transients.
*   **Analog-to-Digital Conversion (ADC):** Hardware-triggered ADC peripheral is utilized for high-speed, accurate voltage sampling during the brief relay closure windows.
*   **Fault Handling:** If calculated resistance drops below the critical threshold (e.g., 500 Ω/V), a hardware interrupt sequence triggers the main contactors to physically isolate the battery pack.
