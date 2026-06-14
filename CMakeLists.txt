cmake_minimum_required(VERSION 3.17)

include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

project(rp2350_mouse_host C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

pico_sdk_init()

add_subdirectory($ENV{PICO_PIO_USB_PATH} pio_usb)

add_executable(rp2350_mouse_host
    src/main.c
    src/usb_descriptors.c
    src/hid_reports.c
    ${PICO_SDK_PATH}/lib/tinyusb/src/portable/raspberrypi/pio_usb/hcd_pio_usb.c
)

target_include_directories(rp2350_mouse_host PRIVATE
    src
)

target_compile_definitions(rp2350_mouse_host PRIVATE
    CFG_TUSB_RHPORT0_MODE=OPT_MODE_DEVICE
    CFG_TUSB_RHPORT1_MODE=OPT_MODE_HOST
    CFG_TUH_RPI_PIO_USB=1
    PICO_DEFAULT_PIO_USB_DP_PIN=16
    PICO_DEFAULT_PIO_USB_VBUSEN_PIN=18
    PICO_DEFAULT_PIO_USB_VBUSEN_STATE=1
    BOARD_TUH_RHPORT=1
)

target_link_libraries(rp2350_mouse_host PRIVATE
    pico_stdlib
    pico_multicore
    tinyusb_host
    tinyusb_device
    tinyusb_board
    pico_pio_usb
    hardware_pio
    hardware_dma
)

pico_add_extra_outputs(rp2350_mouse_host)
