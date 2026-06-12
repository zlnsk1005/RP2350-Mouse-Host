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

target_compile_definitions(rp2350_mouse_host PRIVATE
    PICO_DEFAULT_PIO_USB_DP_PIN=20
    PICO_DEFAULT_PIO_USB_VBUSEN_PIN=22
    PICO_DEFAULT_PIO_USB_VBUSEN_STATE=1
    BOARD_TUH_RHPORT=1
    BOARD_TUD_RHPORT=0
)

target_link_libraries(rp2350_mouse_host
    pico_stdlib
    pico_multicore
    tinyusb_host
    tinyusb_device
    tinyusb_board
    hardware_pio
    hardware_dma
)

pico_add_extra_outputs(rp2350_mouse_host)
