# AgriNode-ESP32-LoRa: Edge-Computed Plant Disease Forecasting

> **Master's Thesis Project:** Low-Power LoRa Sensor Network with Edge
Machine Learning for Crop Disease Risk
Assessment.

## 📌 Overview
This repository contains the firmware for an ultra-low-power, edge-computing IoT sensor node (Node A) designed for precision agriculture. Built on the **Espressif ESP-IDF** framework, the system actively monitors microclimate conditions to calculate the **Smith Period**—a critical agronomic model used for forecasting plant diseases like potato blight.

Instead of transmitting raw sensor data continuously, this node processes the Smith Period algorithm "on the edge," compresses the data into a strict 11-byte payload, and transmits it via a Point-to-Point (P2P) LoRa network, maximizing both battery life and RF bandwidth.

## ⚙️ Hardware Architecture
The physical prototype is optimized for long-term field deployment using a single 18650 Lithium-Ion battery.
* **Microcontroller:** ESP32-C3 (Main state machine and agronomy processing)
* **LoRa Radio:** Seeed Studio Wio-E5 Mini (UART AT-Command driven, 868 MHz)
* **Microclimate Sensor:** Sensirion SHT31-D via I2C (Temperature & Relative Humidity)
* **Leaf Wetness Sensor:** Davis 6420 via ADC (Analog Grid)
* **Soil Moisture Sensor:** Capacitive V1.2 via ADC (Analog Root-Zone)
* **Power Management:** IRLZ44N Logic-Level MOSFET (Hardware power-gating during Deep Sleep)

## 💻 Software Architecture
The firmware utilizes a **Component-Based Architecture** with a dedicated **Board Support Package (BSP)** to decouple hardware pinouts from pure sensor logic. This ensures the component libraries are 100% hardware-agnostic and reusable.

### Directory Structure
```text
Node_A_Project/
├── main/
│   ├── main.c                     # Finite State Machine
│   └── logic_smith_period.c       # Agronomy Math
└── components/
    ├── bsp/                       # Board Support Package (GPIO mappings)
    ├── sht31/                     # Hardware-agnostic I2C driver
    ├── davis_6420/                # ADC leaf wetness driver
    ├── soil_moisture/             # ADC soil calibration driver
    └── wio_e5/                    # UART driver for AT commands
