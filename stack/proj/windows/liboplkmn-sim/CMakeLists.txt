################################################################################
#
# CMake file for openPOWERLINK MN library on Windows with simulation interface
#
# Copyright (c) 2016, B&R Industrial Automation GmbH
# Copyright (c) 2016, Franz Profelt (franz.profelt@gmail.com)
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in the
#       documentation and/or other materials provided with the distribution.
#     * Neither the name of the copyright holders nor the
#       names of its contributors may be used to endorse or promote products
#       derived from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
# ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
# WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
# DISCLAIMED. IN NO EVENT SHALL COPYRIGHT HOLDERS BE LIABLE FOR ANY
# DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
# (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
# ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
# SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
################################################################################

# Set library name
SET(LIB_NAME "oplkmn-sim")
MESSAGE(STATUS "Configuring ${LIB_NAME}")

# Set type of library
IF(CFG_WINDOWS_DLL)
    SET(LIB_TYPE "SHARED")
ELSE()
    SET(LIB_TYPE "STATIC")
ENDIF()

# set general sources of POWERLINK library
SET (LIB_SOURCES
     ${USER_SOURCES}
     ${SDO_SIM_SOURCES}
     ${USER_MN_SOURCES}
     ${CTRL_UCAL_DIRECT_SOURCES}
     ${DLL_UCAL_CIRCBUF_SOURCES}
     ${ERRHND_UCAL_LOCAL_SOURCES}
     ${EVENT_UCAL_SIM_SOURCES}
     ${PDO_UCAL_LOCAL_SOURCES}
     ${USER_TIMER_SIM_SOURCES}
     ${KERNEL_SOURCES}
     ${CTRL_KCAL_DIRECT_SOURCES}
     ${DLL_KCAL_CIRCBUF_SOURCES}
     ${ERRHND_KCAL_LOCAL_SOURCES}
     ${EVENT_KCAL_SIM_SOURCES}
     ${PDO_KCAL_LOCAL_SOURCES}
     ${HARDWARE_DRIVER_SIM_SOURCES}
     ${COMMON_SOURCES}
     ${COMMON_SIM_SOURCES}
     ${TARGET_SIM_SOURCES}
     ${CIRCBUF_SIM_SOURCES}
     ${ARCH_X86_SOURCES}
     ${MEMMAP_NULL_SOURCES}
     ${SIM_IF_SOURCES}
     )

# Configure compile definitions
IF(CFG_INCLUDE_MN_REDUNDANCY)
    ADD_DEFINITIONS(-DCONFIG_INCLUDE_NMT_RMN)
ENDIF()
ADD_DEFINITIONS(-DCONFIG_MN)
ADD_DEFINITIONS(-D_CRT_SECURE_NO_WARNINGS)

# Additional include directories
INCLUDE_DIRECTORIES(
    .
	${SIM_INCLUDE_DIR}
    )

# Group Source and Header files
SOURCE_GROUP("Source Files\\User Source Files" FILES
    ${USER_SOURCES}
    ${USER_MN_SOURCES}
    ${CTRL_UCAL_DIRECT_SOURCES}
    ${DLL_UCAL_CIRCBUF_SOURCES}
    ${ERRHND_UCAL_LOCAL_SOURCES}
    ${EVENT_UCAL_SIM_SOURCES}
    ${PDO_UCAL_LOCAL_SOURCES}
    ${USER_TIMER_SIM_SOURCES}
    )

SOURCE_GROUP("Source Files\\Kernel Source Files" FILES
    ${KERNEL_SOURCES}
    ${CTRL_KCAL_DIRECT_SOURCES}
    ${DLL_KCAL_CIRCBUF_SOURCES}
    ${ERRHND_KCAL_LOCAL_SOURCES}
    ${EVENT_KCAL_SIM_SOURCES}
    ${PDO_KCAL_LOCAL_SOURCES}
    ${HARDWARE_DRIVER_SIM_SOURCES}
    )

SOURCE_GROUP("Source Files\\Common Source Files" FILES
    ${COMMON_SOURCES}
    ${COMMON_SIM_SOURCES}
    ${TARGET_SIM_SOURCES}
    ${CIRCBUF_SIM_SOURCES}
    ${MEMMAP_NULL_SOURCES}
    )

SOURCE_GROUP("Source Files\\Architecture Source Files" FILES
    ${ARCH_X86_SOURCES}
    )

SOURCE_GROUP("Header Files\\External Header Files" FILES ${OPLK_HEADERS})
SOURCE_GROUP("Header Files\\Internal Header Files" FILES ${STACK_HEADERS})
SOURCE_GROUP("Header Files\\Kernel Header Files" FILES ${KERNEL_HEADERS})
SOURCE_GROUP("Header Files\\User Header Files" FILES ${USER_HEADERS})
SOURCE_GROUP("Object Dictionary" FILES
    ${OBJDICT_HEADERS}
    ${OBJDICT_DIR}/${OBJDICT}/objdict.h
    )

# Define library and installation rules
ADD_LIBRARY(${LIB_NAME} ${LIB_TYPE} ${LIB_SOURCES})
TARGET_LINK_LIBRARIES(${LIB_NAME} ${LINK_LIBRARIES})
SET_PROPERTY(TARGET ${LIB_NAME} PROPERTY COMPILE_DEFINITIONS_DEBUG DEBUG;DEF_DEBUG_LVL=${CFG_DEBUG_LVL})
SET_PROPERTY(TARGET ${LIB_NAME} PROPERTY DEBUG_POSTFIX "_d")
INSTALL(TARGETS ${LIB_NAME} RUNTIME DESTINATION . ARCHIVE DESTINATION .)
