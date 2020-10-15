cmake_minimum_required(VERSION 2.8.4)


### Compiler settings
set( CMAKE_CXX_FLAGS_DEBUG "-g -O0" )
set( CMAKE_CXX_FLAGS_RELEASE "-O3" )
set( CMAKE_CXX_STANDARD_REQUIRED ON ) #...is required...
set( CMAKE_CXX_EXTENSIONS ON ) #...with compiler extensions like gnu++11

set( CMAKE_VERBOSE_MAKEFILE ON )


## Include header
include_directories(${PROJECT_SOURCE_DIR}/include)

## Add cuda library
set( CMAKE_CUDA_COMPILER "/usr/local/cuda/bin/nvcc")
find_package(CUDA)
if(NOT CUDA_FOUND)
    message(FATAL_ERROR "CUDA not found")
else()
    message(STATUS "LOG: FIND CUDA")
    enable_language("CUDA")
endif()

## SRC Target
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
file(GLOB CPP_FILES *.cu)
foreach(SRC_SOURCE ${CPP_FILES})
    string(REGEX REPLACE ".+/(.+)\\..*" "\\1" SRC_NAME ${SRC_SOURCE})
    message(STATUS "LOGO: ${SRC_NAME} ${SRC_SOURCE}")
    cuda_add_library(${SRC_NAME} ${SRC_SOURCE})
    target_link_libraries(${SRC_NAME}  ${CUDA_LIBRARIES})
    set_target_properties(${SRC_NAME} PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
    message(STATUS "LOGO: generate lib success, the output dir is : ${LIBRARY_OUTPUT_PATH}")
endforeach(SRC_SOURCE)

