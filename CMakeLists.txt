cmake_minimum_required(VERSION 3.13)

# initialize the SDK directly
include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

#seting C and C++ standards
set(CMAKE_C_COMPILER "arm-none-eabi-gcc")
set(CMAKE_CXX_COMPILER "arm-none-eabi-g++")

set(C_STANDARD 11)
set(Cxx_STANDARD 17)

project(RP2040_DS3231 C CXX ASM)
message("\nProject name is ${PROJECT_NAME}\n")

# initialize the Raspberry Pi Pico SDK
pico_sdk_init()

add_library(${PROJECT_NAME} RP2040_DS3231.cpp RP2040_DS3231.hpp)

target_include_directories(${PROJECT_NAME} INTERFACE ${CMAKE_CURRENT_LIST_DIR})


# Add pico_stdlib library which aggregates commonly used features
target_link_libraries(${PROJECT_NAME} hardware_i2c pico_stdlib)
