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

target_include_directories(rp2350_mouse_host PRIVATE src)

# Força o USB Host via PIO para o RP2350 e usa os pinos corretos (DP=16, DM=17)
target_compile_definitions(rp2350_mouse_host PRIVATE
    CFG_TUH_RPI_PIO_USB=1
    PICO_DEFAULT_PIO_USB_DP_PIN=16
)

# Adiciona a biblioteca pico_pio_usb que faltava para compilar os hcd_*
target_link_libraries(rp2350_mouse_host
    pico_stdlib
    pico_multicore
    tinyusb_device
    tinyusb_host
    tinyusb_board
    pico_pio_usb
)

pico_add_extra_outputs(rp2350_mouse_host)
