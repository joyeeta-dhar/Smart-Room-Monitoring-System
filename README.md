# 🏠 Smart Room Monitor (STM32-Based)

[![Status](https://img.shields.io/badge/Status-Completed-success)](https://github.com/)
[![Platform](https://img.shields.io/badge/Platform-STM32CubeIDE-blue)](https://www.st.com/en/development-tools/stm32cubeide.html)
[![MCU](https://img.shields.io/badge/MCU-STM32F103C8T6%20(Blue%20Pill)-orange)](https://www.st.com/en/microcontrollers-microprocessors/stm32f103c8.html)

An automated environmental monitoring and safety system built on the **STM32F103C8T6**. This project integrates multi-sensor data to manage room lighting and provide real-time fire safety alerts.

---

## 🚀 Key Features
* **Environmental Tracking**: Real-time Temperature & Humidity monitoring via **DHT11**.
* **Automated Lighting**: Intelligent LED control using an **LDR** (Light Dependent Resistor) and ADC.
* **Fire Safety**: High-priority flame detection with a physical **Buzzer** alarm and OLED visual warnings.
* **OLED Dashboard**: 128x64 **SSD1306** display interface for system status and sensor readings.
* **Efficient Firmware**: Optimized using STM32 HAL drivers and microsecond-precision timers.

---

## 🛠️ Hardware Mapping
| Component | Function | Pin (STM32) | Mode |
| :--- | :--- | :--- | :--- |
| **LDR Sensor** | Ambient Light Sense | **PA0** | ADC1_IN0 |
| **DHT11** | Temp & Humidity | **PA1** | GPIO_Input (Custom 1-Wire) |
| **Flame Sensor** | Fire Detection | **PA2** | GPIO_Input (Active-Low) |
| **LED Anode** | Room Light Output | **PA3** | GPIO_Output |
| **Buzzer** | Alarm Output | **PA8** | GPIO_Output |
| **OLED SCL** | Display Clock | **PB6** | I2C1_SCL |
| **OLED SDA** | Display Data | **PB7** | I2C1_SDA |

---

## 📊 Logic Flow
1.  **Initialize**: Setup HAL, System Clock, GPIOs, ADC, I2C, and TIM1 (for DHT11 delays).
2.  **Sense**:
    * `DHT11_Read()`: Collects humidity and temperature.
    * `HAL_ADC_GetValue()`: Samples the light level.
    * `HAL_GPIO_ReadPin()`: Checks for fire (0 = Fire detected).
3.  **Act**:
    * If **LDR < 1500**: PA3 (LED) → **HIGH**.
    * If **Flame == RESET**: PA8 (Buzzer) → **HIGH** & OLED displays **"!!! FIRE !!!"**.
4.  **Display**: Refresh OLED with formatted sensor strings.

---

## 💻 Software Setup
1.  Clone the repository
2.  Open the project in **STM32CubeIDE**.
3.  Ensure **TIM1** is enabled (Internal Clock, Prescaler 71) to handle the DHT11 timing.
4.  Build project and flash via **ST-LINK V2**.

---

## ⚠️ Troubleshooting
* **Target Not Found**: Ensure ST-LINK is in "Connect Under Reset" mode in Debug Configurations.
* **OLED Blank**: Check I2C pull-up resistors and device address (usually `0x3C`).
* **LED Not Lighting**: Verify PA3 is connected to the Anode (+) and the Cathode (-) is grounded.

---

## 👤 Author
**Name:** Joyeeta Dhar   
**Project Date:** 21 April 2026

---
*Developed for STM32 Microcontroller Laboratory.*
