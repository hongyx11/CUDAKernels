cmake_minimum_required(VERSION 3.19)
project(NVIDIA_SGEMM_PRACTICE LANGUAGES CXX CUDA)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

find_package(CUDAToolkit REQUIRED)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CUDA_ARCHITECTURES 80 86 90)

# add dependencies
include(cmake/CPM.cmake)

CPMAddPackage("gh:gabime/spdlog@1.8.2")
link_libraries(spdlog::spdlog)
# in debug mode, add debug symbols to device code
# this disables most optimizations and kills performance
# add_compile_options("$<$<AND:$<CONFIG:Debug>,$<COMPILE_LANGUAGE:CUDA>>:-G;-src-in-ptx>")
# add_compile_options("--ptxas-options=-v")

# Configure header file search paths
# include_directories(${CUDA_INCLUDE_DIRS})
# include_directories(${PROJECT_SOURCE_DIR}/src)
# Configure the source file path to be compiled
aux_source_directory(${PROJECT_SOURCE_DIR}/src SRC)

# generate executable
add_executable(sgemm sgemm.cu ${SRC})
target_link_libraries(sgemm CUDA::cudart CUDA::cublas)

add_executable(cuBLAS_sgemm cuBLAS_sgemm.cu )
target_link_libraries(cuBLAS_sgemm CUDA::cudart CUDA::cublas)

add_executable(simplest_kernel simplest_kernel.cu)
target_link_libraries(simplest_kernel CUDA::cudart CUDA::cublas)