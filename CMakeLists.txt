cmake_minimum_required(VERSION 2.6)

project(crnn)
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_BUILD_TYPE Debug CACHE STRING "set build type to debug")

#add_definitions(-std=c++11)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(CUDA_USE_STATIC_CUDA_RUNTIME OFF)
#set(CMAKE_CXX_STANDARD 11)
set(CMAKE_BUILD_TYPE Debug)

# cuda10.2
include_directories(/usr/local/cuda/include)
link_directories(/usr/local/cuda/lib64)

###libtorch1.7
set(TORCH_ROOT ${CMAKE_SOURCE_DIR}/3rdparty/libtorch1.7)
set(CMAKE_PREFIX_PATH ${PROJECT_SOURCE_DIR}/3rdparty/libtorch1.7)
include_directories(${TORCH_ROOT}/include)
include_directories(${TORCH_ROOT}/include/torch/csrc/api/include)
link_directories(${TORCH_ROOT}/lib)

#OpenCv3.4.6
set(OPENCV_ROOT ${CMAKE_SOURCE_DIR}/3rdparty/opencv3.4.6)
include_directories(${OPENCV_ROOT}/include)
link_directories(${OPENCV_ROOT}/lib)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall -Ofast -Wfatal-errors -D_MWAITXINTRIN_H_INCLUDED")

add_executable(crnn ${PROJECT_SOURCE_DIR}/crnn.cpp)
target_link_libraries(crnn opencv_calib3d opencv_core opencv_dnn opencv_imgproc opencv_highgui opencv_imgcodecs)
target_link_libraries(crnn c10 c10_cuda torch torch_cuda torch_cpu "-Wl,--no-as-needed -ltorch_cuda")

add_definitions(-O2 -pthread)
