# Set minimum required version of CMake
cmake_minimum_required(VERSION 3.12)

# Include build functions from Pico SDK
include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

# Set name of project (as PROJECT_NAME) and C/C++ standards
project(adc_fft C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Creates a pico-sdk subdirectory in our project for the libraries
pico_sdk_init()

# Tell CMake where to find the executable source file
add_executable(${PROJECT_NAME} 
    adc_fft.c
)

add_library(kiss_fftr kiss_fftr.c)
add_library(kiss_fft kiss_fft.c)

target_link_libraries(kiss_fftr kiss_fft)

pico_enable_stdio_usb(adc_fft 1)
pico_enable_stdio_uart(adc_fft 1)

# Create map/bin/hex/uf2 files
pico_add_extra_outputs(${PROJECT_NAME})

# Link to pico_stdlib (gpio, time, etc. functions)
target_link_libraries(${PROJECT_NAME} 
    pico_stdlib
	hardware_adc
	hardware_dma
	kiss_fftr
)
