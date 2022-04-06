cmake_minimum_required(VERSION 3.0.0)
project(etcpak VERSION 0.8)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

set(TARGET_STATIC etcpak-static)

add_library(${TARGET_STATIC}
    STATIC
        Dither.cpp
        ProcessRGB.cpp
        Tables.cpp)

target_include_directories(${TARGET_STATIC}
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:.>)

target_compile_options(${TARGET_STATIC}
    PRIVATE
        $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-msse4.1 -mavx2 -mfma>
        $<$<CXX_COMPILER_ID:MSVC>:/arch:AVX2>
        $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-Wno-logical-not-parentheses>)
