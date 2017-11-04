cmake_minimum_required(VERSION 2.8.4)
project(rhessys)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -Wall -std=c99")
set(RHESSYS_SOURCE_DIR "C:/z_code/Hydro/RHESSys/rhessys")
set(PROJECT_BINARY_DIR "C:/z_code/Hydro/RHESSys/bin")
add_custom_target(rhessys COMMAND make -C ${RHESSYS_SOURCE_DIR}
                         RHESSYS_BIN=${PROJECT_BINARY_DIR})