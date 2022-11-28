cmake_minimum_required(VERSION 3.12)

# Pull in SDK (must be before project)
include(pico_sdk_import.cmake)

project(lcd-lib C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialize the SDK
pico_sdk_init()

if (TARGET tinyusb_device)
	add_executable(hello_usb
			hello_usb.c
			regexp.c
			regsub.c
		)

	# Pull in our pico_stdlib which aggregates commonly used features
	target_link_libraries(hello_usb pico_stdlib)

	# enable usb output, disable uart output
	pico_enable_stdio_usb(hello_usb 1)
	pico_enable_stdio_uart(hello_usb 0)

	# create map/bin/hex/uf2 file etc.
	pico_add_extra_outputs(hello_usb)

elseif(PICO_ON_DEVICE)
	message(WARNING "not building hello_usb because TinyUSB submodule is not initialized in the SDK")
endif()
