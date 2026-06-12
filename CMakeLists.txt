cmake_minimum_required(VERSION 3.17)

include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

project(rp2350_mouse_host C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

pico_sdk_init()

add_executable(rp2350_mouse_host
    src/main.c
    src/usb_descriptors.c
    src/hid_reports.c
)

target_include_directories(rp2350_mouse_host PRIVATE
    src
)

target_link_libraries(rp2350_mouse_host
    pico_stdlib
    tinyusb_host
    tinyusb_board
)

pico_add_extra_outputs(rp2350_mouse_host)
