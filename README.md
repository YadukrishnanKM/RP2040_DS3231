# RP2040_DS3231

A lightweight and easy-to-use C++ library for interfacing the DS3231 Real-Time Clock (RTC) module with the Raspberry Pi Pico (RP2040) using the Pico SDK and I2C interface.

## 📦 Features

- Set and retrieve time in both 12-hour and 24-hour formats
- Configure and manage alarms (Alarm 1 & Alarm 2)
- Read on-chip temperature from DS3231
- Enable/disable and check the status of control bits (EOSC, OSF, INTCN, etc.)
- Control square wave output frequency
- Read/write from control and status registers
- BCD conversion helpers
- Optimized for memory-constrained environments

---

## 📁 Project Structure

RP2040_DS3231/

├── CMakeLists.txt # CMake configuration for building the static library

├── rp2040_ds3231.hpp # Main library header

├── rp2040_ds3231.cpp # (source)

└── README.md


---

## 🚀 Getting Started

## Installation
1. Clone this repository into your Pico SDK project:
   ```powershell
   git clone https://github.com/YadukrishnanKM/RP2040_DS3231.git
   ```
2. Create a lib folder and add this `CMakeLists.txt` in it
   ```cmake
   add_subdirectory(RP2040_DS3231)
   add_library(lib INTERFACE)
   target_link_libraries(lib INTERFACE RP2040_DS3231)
   target_include_directories(lib INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/Liquid_Crystal_RP)
   ```

3. Add the library to your `CMakeLists.txt`:
   ```cmake
   add_subdirectory(lib)
   target_link_libraries(your_project_name PRIVATE RP2040_DS3231)
   ```
4. Include the header in your project:
   ```cpp
   #include "RP2040_DS3231.hpp"
   ```

## CMake Configuration
To build this library using the Raspberry Pi Pico SDK, ensure your `CMakeLists.txt` file includes the following:

```cmake
cmake_minimum_required(VERSION 3.13)
include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

project(LiquidCrystal_I2C_Pico)

pico_sdk_init()

add_executable(${PROJECT_NAME} src/main.cpp)

add_subdirectory(lib)

# Link with Pico SDK
target_link_libraries(${PROJECT_NAME} pico_stdlib RP2040_DS3231)
```
## API Reference
```cpp
#include "rp2040_ds3231.hpp"

i2c_init(i2c1, 100 * 1000);  // Init I2C1 at 100kHz
gpio_set_function(2, GPIO_FUNC_I2C); // SDA
gpio_set_function(3, GPIO_FUNC_I2C); // SCL

DS_3231 rtc(0x68);  // DS3231 default I2C address
```


---

## ⏱️ Core Time Functions

| Method                                                 | Description                     |
| ------------------------------------------------------ | ------------------------------- |
| `set_time_12_hr(uint8_t *time)`                        | Set RTC time in 12-hour format. |
| `set_time_24_hr(uint8_t *time)`                        | Set RTC time in 24-hour format. |
| `get_time(uint8_t *dest)`                              | Fetch time (7-byte BCD array).  |
| `get_time_from_bcd(uint8_t *bcd_time, uint8_t length)` | Convert BCD to binary.          |

---

## ⏰ Alarm Functions

| Method                                                           | Description                     |
| ---------------------------------------------------------------- | ------------------------------- |
| `set_Alarm(bool alarm, bool DY_DT, bool hrs_fmt, uint8_t *data)` | Set Alarm 1 (0) or Alarm 2 (1). |
| `Alarm_Bits_Mask_1(uint8_t type)`                                | Set alarm 1 masking bits.       |
| `Alarm_Bits_Mask_2(uint8_t type)`                                | Set alarm 2 masking bits.       |

---

## 🌡️ Sensor + Control

| Method                               | Description                            |
| ------------------------------------ | -------------------------------------- |
| `int8_t read_temp()`                 | Read temperature from internal sensor. |
| `int8_t read_Aging_Offset()`         | Read aging offset.                     |
| `void control_write(uint8_t)`        | Write to Control register (0x0E).      |
| `void control_status_write(uint8_t)` | Write to Control/Status (0x0F).        |

---

## 🔁 Bit Control APIs

### General Bits

| Method                              | Description                  |
| ----------------------------------- | ---------------------------- |
| `status_EOSC()`                     | Enable Oscillator Stop Flag. |
| `status_osf()`                      | Oscillator Stop Flag status. |
| `status_EN32kHz()`                  | 32kHz output status.         |
| `set_EN32kHz()` / `reset_EN32kHz()` | Toggle 32kHz output.         |

### Square Wave Output

| Method                           | Description                                                     |
| -------------------------------- | --------------------------------------------------------------- |
| `set_sq_wave_freq(uint8_t type)` | Set square wave output freq. Refer datasheet for `type` values. |

### Alarm Interrupts

| Bit   | Methods                                          |
| ----- | ------------------------------------------------ |
| A1IE  | `status_A1IE()`, `set_A1IE()`, `reset_A1IE()`    |
| A2IE  | `status_A2IE()`, `set_A2IE()`, `reset_A2IE()`    |
| INTCN | `status_INTCN()`, `set_INTCN()`, `reset_INTCN()` |
| BBSQW | `status_BBSQW()`, `set_BBSQW()`, `reset_BBSQW()` |
| CONV  | `status_CONV()`, `set_CONV()`, `reset_CONV()`    |

---

## 🧠 Memory

```cpp
uint8_t *general_purpose_memory_location;
```

A user-accessible general-purpose memory buffer (5 bytes). Optional use, ensure memory safety.

---

## 🧾 Notes

* Make sure I2C pins are correctly initialized before using the RTC.
* All time arrays passed to or from the library are **7 bytes long**.
* Consult the **DS3231 datasheet** for alarm types and control bit configurations.

---

## 📚 References

* [DS3231 Datasheet (Maxim)](https://datasheets.maximintegrated.com/en/ds/DS3231.pdf)
* [Raspberry Pi Pico SDK](https://github.com/raspberrypi/pico-sdk)

---

## 📄 License

MIT License — *Free to use with attribution.*

---
