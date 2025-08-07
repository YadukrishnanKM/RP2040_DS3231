file(GLOB SOURCES "*.cpp")

add_library(RP2040_DS3231 STATIC ${SOURCES})
target_include_directories(RP2040_DS3231 PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(RP2040_DS3231 PUBLIC hardware_i2c
                                                pico_stdlib
                                                )