#=============================================================================#
# Author: QueezyTheGreat                                                      #
# Date:   26.04.2011                                                          #
#                                                                             #
# Description: Arduino CMake example                                          #
#                                                                             #
#=============================================================================#
set(CMAKE_TOOLCHAIN_FILE arduino-cmake/cmake/ArduinoToolchain.cmake) # Arduino Toolchain


cmake_minimum_required(VERSION 2.8)
#====================================================================#
#  Setup Project                                                     #
#====================================================================#
project(ArduinoTests C CXX)

option(DEBUG "Enable debug" OFF)

#add_definitions(-D__AVR_LIBC_DEPRECATED_ENABLE__)
#add_definitions(-D__PROG_TYPES_COMPAT__)
#add_definitions(-Wno-deprecated-declarations)
#add_definitions(-Wno-reorder)
#add_definitions(-Wno-unused-function)
#add_definitions(-Wno-unused-variable)

# modify flags from default toolchain flags
set(APM_OPT_FLAGS "-Wformat -Wall -Wshadow -Wpointer-arith -Wcast-align -Wwrite-strings -Wformat=2")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${APM_OPT_FLAGS}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${APM_OPT_FLAGS} -Wno-reorder")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${APM_OPT_FLAGS} -Wl,--relax")

# debug output
if (DEBUG)
    arduino_debug_on()
endif()

add_subdirectory(arduino-cmake)

generate_arduino_firmware(Blink2
    SKETCH ${CMAKE_SOURCE_DIR}/test/Blink2
    BOARD mega
    PORT /dev/ttyUSB0)

set(Blink3_SKETCH ${CMAKE_SOURCE_DIR}/test/Blink3)
set(Blink3_BOARD mega)
set(Blink3_PORT /dev/ttyUSB0)
generate_arduino_firmware(Blink3)

generate_arduino_firmware(ArduPlane
    SKETCH ${CMAKE_SOURCE_DIR}/test/ArduPlane
    BOARD mega
    PORT /dev/ttyUSB0)

generate_arduino_firmware(ArduCopter
    SKETCH ${CMAKE_SOURCE_DIR}/test/ArduCopter
    BOARD mega
    PORT /dev/ttyUSB0)
