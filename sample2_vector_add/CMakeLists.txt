cmake_minimum_required(VERSION 3.0.0)

project(vector_add LANGUAGES CXX)

## 1. Set CUDA package
set( CMAKE_CUDA_COMPILER "/usr/local/cuda/bin/nvcc")
find_package(CUDA)
if(NOT CUDA_FOUND)
    message(FATAL_ERROR "CUDA not found")
else()
    message(STATUS "LOG: FIND CUDA")
    enable_language("CUDA")
endif()

#### 1.1 Include header
include_directories(/usr/local/cuda-10.0/include)

## 2. Add cuda lib
cuda_add_library(vector_add SHARED add.cu add.hpp)
target_compile_features(vector_add PUBLIC cxx_std_11)
target_link_libraries(vector_add  ${CUDA_LIBRARIES})
set_target_properties(vector_add PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

## 3. Add exe
#find_library(ADD_LIB NAMES vector_add PATHS ${PROJECT_BINARY_DIR})
#message("add lib is : " ${ADD_LIB})
add_executable(vector_add_sample main.cpp)
#target_link_libraries(vector_add_sample PUBLIC ADD_LIB)
target_link_libraries(vector_add_sample vector_add)
target_include_directories(vector_add_sample PUBLIC ${PROJECT_SOURCE_DIR})

set_target_properties(vector_add_sample PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

## 4. Micro defination
option(USE_CUDA "use cuda kernel" ON)
message(STATUS "USE_CUDA: ${USE_CUDA}")
if(USE_CUDA)
    add_definitions(-DUSE_CUDA)
endif()

## 5.Make install
set(CMAKE_INSTALL_PREFIX ${PROJECT_BINARY_DIR})
install(TARGETS vector_add DESTINATION lib)
install(TARGETS vector_add_sample DESTINATION bin)
install(FILES add.hpp DESTINATION include)

## 6.How to run bin: Export lib path to LD_LIBRARY_PATH
# export LD_LIBRARY_PATH=${PROJECT_BINARY_DIR}/lib"$LD_LIBRARY_DIR