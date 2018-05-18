cmake_minimum_required(VERSION 2.8.4)
project(rhessys)

add_custom_target(rhessys COMMAND make -C ${CMAKE_CURRENT_SOURCE_DIR}
                  RHESSYS_BIN=${PROJECT_BINARY_DIR})