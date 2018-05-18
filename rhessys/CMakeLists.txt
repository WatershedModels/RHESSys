cmake_minimum_required(VERSION 2.8.4)
project(rhessys)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -Wall -std=c99")
set(SOURCE_DIR .)
add_custom_target(rhessys COMMAND mingw32-make -C ${SOURCE_DIR}
        RHESSYS_BIN=${PROJECT_BINARY_DIR})