cmake_minimum_required(VERSION 3.5.0)
project(etcpak VERSION 1.0)

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

if($<CXX_COMPILER_ID:MSVC>)
    target_compile_options(${TARGET_STATIC}
        PRIVATE
            /arch:AVX2)
else()
    if($<CMAKE_HOST_SYSTEM_PROCESSOR:x86_64>)
        target_compile_options(${TARGET_STATIC}
            PRIVATE
                -msse4.1 -mavx2 -mfma)
    endif()

    target_compile_options(${TARGET_STATIC}
        PRIVATE
            -Wno-logical-not-parentheses)
endif()
