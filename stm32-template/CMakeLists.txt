PROJECT(stm32-template)

CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
ENABLE_LANGUAGE(ASM)

LIST(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/../cmake/Modules)

FIND_PACKAGE(CMSIS REQUIRED)

STM32_SET_PARAMS("512K" "64K" "0x20010000")
SET(CMAKE_EXE_LINKER_FLAGS "-T${CMAKE_CURRENT_BINARY_DIR}/stm32_flash.ld ${CMAKE_EXE_LINKER_FLAGS}")

INCLUDE_DIRECTORIES(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMSIS_INCLUDE_DIR}
    ${StdPeriphLib_INCLUDE_DIR}
)

SET(PROJECT_SOURCES
    main.c
)

ADD_EXECUTABLE(${CMAKE_PROJECT_NAME}.elf ${PROJECT_SOURCES} ${CMSIS_STARTUP_SOURCE})
TARGET_LINK_LIBRARIES(${CMAKE_PROJECT_NAME}.elf ${CMSIS_LIBRARIES} ${StdPeriphLib_LIBRARIES})

ADD_CUSTOM_TARGET(${CMAKE_PROJECT_NAME}.hex DEPENDS ${CMAKE_PROJECT_NAME}.elf COMMAND ${CMAKE_OBJCOPY} -Oihex ${CMAKE_PROJECT_NAME}.elf ${CMAKE_PROJECT_NAME}.hex)
ADD_CUSTOM_TARGET(${CMAKE_PROJECT_NAME}.bin DEPENDS ${CMAKE_PROJECT_NAME}.elf COMMAND ${CMAKE_OBJCOPY} -Obinary ${CMAKE_PROJECT_NAME}.elf ${CMAKE_PROJECT_NAME}.bin)
