cmake_minimum_required(VERSION 2.8.4)

project(cuda_samples)

## Set output dirs of installation, bin, and lib prefix
set(CMAKE_INSTALL_PREFIX ${PROJECT_BINARY_DIR}/build/Install/lib)
#SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
#SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)


## Install
set(INSTALL_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/include)
install(DIRECTORY "${INSTALL_INCLUDE_DIR}"
        DESTINATION include
        FILES_MATCHING PATTERN "*.h"
        )

## Add include directories and sbu-dir
#include_directories(include)
add_subdirectory(src)
add_subdirectory(samples)
