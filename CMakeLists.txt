cmake_minimum_required(VERSION 3.30 FATAL_ERROR)
set(CMAKE_CXX_EXTENSIONS off)

foreach(
    level
    MAJOR
    MINOR
    PATCH
)
    file(
        STRINGS "${CMAKE_CURRENT_SOURCE_DIR}/source/core/version.hpp"
        cyan_${level}
        REGEX "^#define CYAN_${level}_VERSION [0-9]+$"
    )
    if(NOT cyan_${level})
        message(FATAL_ERROR "Unable to determine Cyan's ${level} version")
    endif()
    string(
        REGEX MATCH "([0-9]+)"
        cyan_version_${level}
        "${cyan_${level}}"
    )
endforeach()

project(
    cyan VERSION "${cyan_version_MAJOR}.${cyan_version_MINOR}.${cyan_version_PATCH}"
    LANGUAGES CXX
)
include(CTest)

add_library(cyan INTERFACE)
add_library(Cyan::Cyan ALIAS cyan)

if(MSVC)
    target_compile_options(cyan INTERFACE
        /options:strict
        /bigobj
        /utf-8
        /permissive-
        /Zc:preprocessor
        /Zc:__cplusplus
        /Zc:checkGwOdr
    )
endif()

target_compile_definitions(cyan INTERFACE
    $<$<CONFIG:Debug>:
        CYAN_DEBUG=
    >
)

target_include_directories(cyan INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}/source")
target_compile_features(cyan INTERFACE cxx_std_26)

if(BUILD_TESTING)
    add_subdirectory(test)
endif()

if(BUILD_BENCHMARKING)
    add_subdirectory(benchmark)
endif()