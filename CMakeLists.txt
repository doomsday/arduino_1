cmake_minimum_required(VERSION 3.16)
set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_SYSTEM_VERSION 1)
project(arduino_1 C CXX ASM)

###################################
#  Machine-specific stuff
set(AVR_DUDE_EXECUTABLE C:/tools/avrdude-6.4-mingw32/avrdude.exe)
set(AVR_DUDE_PORT COM4)
#  End of machine-specific stuff
###################################

set(CMAKE_CXX_STANDARD 11)
include(CMakeVerifyCompiler.cmake)

set(MCU atmega328p)

include_directories(lib/arduino/core lib/arduino/variants/standard)
file(GLOB_RECURSE SOURCES "src/*.*"  "lib/*.*")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-exceptions -fno-threadsafe-statics -fpermissive" )
add_compile_options(-c  -Os -Wall -ffunction-sections -fdata-sections -flto -mmcu=${MCU})
add_compile_definitions(ARDUINO_AVR_UNO;F_CPU=16000000L;ARDUINO_ARCH_AVR;ARDUINO=10808)
add_link_options(-Os -flto -fuse-linker-plugin -mmcu=${MCU} -Wl,--gc-sections,--print-memory-usage,-Map=${PROJECT_BINARY_DIR}/${PROJECT_NAME}.map -lm)

add_executable(arduino_1 ${SOURCES})

set(HEX_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.hex)
set(BIN_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.bin)

add_custom_command(TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} -Oihex $<TARGET_FILE:${PROJECT_NAME}> ${HEX_FILE}
        COMMENT "Building ${HEX_FILE}")

add_custom_target(FLASH
        ${AVR_DUDE_EXECUTABLE} -p ${MCU} -c arduino -b115200 -P ${AVR_DUDE_PORT} -v -v -F -D -U flash:w:${PROJECT_BINARY_DIR}/${PROJECT_NAME}:e
        DEPENDS ${PROJECT_NAME}
        COMMENT "Flash to ${TARGET_CPU}")
