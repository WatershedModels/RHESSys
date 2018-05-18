# ----------------------------------------------------------------------------
#  Root CMake file for RHESSys.
#     The Regional Hydro-Ecologic Simulation System
#
#     Please from the off-tree build directory, invoke:
#       $ cmake <path/to/RHESSys>
#
#  Routine testing platforms and compilers include:
#     1. Windows 10 with mingw64 (GCC-4.9.3)
#
#  Created and maintained by Liangjun Zhu (zlj@lreis.ac.cn)
#  Latest updated: Mar. 18, 2018
#
#  Copyright (C) 2018 Lreis, IGSNRR, CAS
# ----------------------------------------------------------------------------
cmake_minimum_required(VERSION 2.8.4)
project(rhessys_all)

### Set cmake find path
set(CMAKE_MODULE_PATH
        ${CMAKE_CURRENT_SOURCE_DIR}/cmake
        ${CMAKE_MODULE_PATH}
        )

### Set default or specified installation directory.
IF (INSTALL_PREFIX)
    SET(CMAKE_INSTALL_PREFIX ${INSTALL_PREFIX} CACHE PATH "Set specified install path" FORCE)
ELSEIF(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    SET(CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR}/bin CACHE PATH "Set default install path" FORCE)
ENDIF ()
SET(INSTALL_DIR ${CMAKE_INSTALL_PREFIX})

### In case of Makefiles if the user does not setup CMAKE_BUILD_TYPE, assume it's Release.
IF(CMAKE_GENERATOR MATCHES "Makefiles|Ninja" AND "${CMAKE_BUILD_TYPE}" STREQUAL "")
    SET(CMAKE_BUILD_TYPE Release)
ENDIF()

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -Wall -std=c99")

### Detect compiler and target platform architecture.
INCLUDE(Utils)

### Add subdirectories.
ADD_SUBDIRECTORY(./cf)
ADD_SUBDIRECTORY(./rhessys)

### Build platform.
STATUS("")
STATUS("  Platform:")
IF(NOT CMAKE_VERSION VERSION_LESS 2.8.11 AND NOT BUILD_INFO_SKIP_TIMESTAMP)
    STRING(TIMESTAMP TIMESTAMP "" UTC)
    IF(TIMESTAMP)
        STATUS("    Timestamp:"  ${TIMESTAMP})
    ENDIF()
ENDIF()
STATUS("    Host:"               ${CMAKE_HOST_SYSTEM_NAME} ${CMAKE_HOST_SYSTEM_VERSION} ${CMAKE_HOST_SYSTEM_PROCESSOR})
IF(CMAKE_CROSSCOMPILING)
    STATUS("    Target:"         ${CMAKE_SYSTEM_NAME} ${CMAKE_SYSTEM_VERSION} ${CMAKE_SYSTEM_PROCESSOR})
ENDIF()
STATUS("    CMake:"              ${CMAKE_VERSION})
STATUS("    CMake generator:"    ${CMAKE_GENERATOR})
STATUS("    CMake build tool:"   ${CMAKE_BUILD_TOOL})
IF(MSVC)
    STATUS("    MSVC:"           ${MSVC_VERSION})
ENDIF()
IF(CMAKE_GENERATOR MATCHES Xcode)
    STATUS("    Xcode:"          ${XCODE_VERSION})
ENDIF()
IF(NOT CMAKE_GENERATOR MATCHES "Xcode|Visual Studio")
    STATUS("    Configuration:"  ${CMAKE_BUILD_TYPE})
ENDIF()

### C/C++ options.
IF(CMAKE_CXX_COMPILER_VERSION)
    SET(GEO_COMPILER_STR "${CMAKE_CXX_COMPILER} ${CMAKE_CXX_COMPILER_ARG1} (ver ${CMAKE_CXX_COMPILER_VERSION})")
ELSE()
    SET(GEO_COMPILER_STR "${CMAKE_CXX_COMPILER} ${CMAKE_CXX_COMPILER_ARG1}")
ENDIF()
STRING(STRIP "${GEO_COMPILER_STR}" GEO_COMPILER_STR)

STATUS("")
STATUS("  C/C++:")
IF(ENABLE_CXX11 OR HAVE_CXX11)
    STATUS("    C++11:" HAVE_CXX11 THEN YES ELSE NO)
ENDIF()
STATUS("    C++ Compiler:"           ${GEO_COMPILER_STR})
STATUS("    C++ flags (Release):"    ${CMAKE_CXX_FLAGS} ${CMAKE_CXX_FLAGS_RELEASE})
STATUS("    C++ flags (Debug):"      ${CMAKE_CXX_FLAGS} ${CMAKE_CXX_FLAGS_DEBUG})
STATUS("    C Compiler:"             ${CMAKE_C_COMPILER} ${CMAKE_C_COMPILER_ARG1})
STATUS("    C flags (Release):"      ${CMAKE_C_FLAGS} ${CMAKE_C_FLAGS_RELEASE})
STATUS("    C flags (Debug):"        ${CMAKE_C_FLAGS} ${CMAKE_C_FLAGS_DEBUG})
IF(WIN32)
    STATUS("    Linker flags (Release):" ${CMAKE_EXE_LINKER_FLAGS} ${CMAKE_EXE_LINKER_FLAGS_RELEASE})
    STATUS("    Linker flags (Debug):"   ${CMAKE_EXE_LINKER_FLAGS} ${CMAKE_EXE_LINKER_FLAGS_DEBUG})
ELSE()
    STATUS("    Linker flags (Release):" ${CMAKE_SHARED_LINKER_FLAGS} ${CMAKE_SHARED_LINKER_FLAGS_RELEASE})
    STATUS("    Linker flags (Debug):"   ${CMAKE_SHARED_LINKER_FLAGS} ${CMAKE_SHARED_LINKER_FLAGS_DEBUG})
ENDIF()

### Auxiliary.
STATUS("")
STATUS("  Install path:" ${INSTALL_DIR})
STATUS("")

