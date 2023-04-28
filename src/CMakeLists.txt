cmake_minimum_required(VERSION 3.10)
# Updated to 3.22 to match the cmake version on CSIF
# Defines the minimum required cmake version
# You can check cmake version on your os by typing 
# `cmake --version` on the terminal

project(hw0 VERSION 1.1.0)
# Project Name and Version

set(INCLUDE_DIR ${PROJECT_SOURCE_DIR}/include)
set(LIB_DIR ${PROJECT_SOURCE_DIR}/lib)
set(CMAKE_BUILD_TYPE ${CMAKE_BUILD_TYPE}
    CACHE STRING "Choose the type of build, options are: None Debug Release asan"
    FORCE)
# AddressSanitize
set(CMAKE_C_FLAGS_ASAN
    "-fsanitize=address -fno-optimize-sibling-calls -fsanitize-address-use-after-scope -fno-omit-frame-pointer -g -O1 -Wall -Wextra -Wpedantic -Werror"
    CACHE STRING "Flags used by the C compiler during AddressSanitizer builds."
    FORCE)
set(CMAKE_CXX_FLAGS_ASAN
    "-fsanitize=address -fno-optimize-sibling-calls -fsanitize-address-use-after-scope -fno-omit-frame-pointer -g -O1 -Wall -Wextra -Wpedantic -Werror"
    CACHE STRING "Flags used by the C++ compiler during AddressSanitizer builds."
    FORCE)

file(GLOB_RECURSE LIB_FILES "${LIB_DIR}/*.c")
file(GLOB_RECURSE HEADER_FILES "${INCLUDE_DIR}/*.h")

include_directories(${INCLUDE_DIR})

set(SOURCE_FILES main.c)
add_executable(Main ${SOURCE_FILES} ${HEADER_FILES})

foreach(lib_file ${LIB_FILES})
    get_filename_component(lib_name ${lib_file} NAME_WE)
    message(${lib_name})
    message(${lib_file})
    add_library(${lib_name} SHARED ${lib_file})
    target_link_libraries(Main ${lib_name})
endforeach()

set_property(TARGET Main PROPERTY C_STANDARD 99)
