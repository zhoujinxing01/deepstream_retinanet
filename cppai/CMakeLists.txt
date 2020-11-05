# 参考：https://www.icode9.com/content-3-518162.html
# 声明要求的 cmake 最低版本
cmake_minimum_required(VERSION 3.9 FATAL_ERROR)

# 声明一个 cmake 工程
project(retinanet_infer LANGUAGES CXX)

# 设置编译模式
#message(${CMAKE_CXX_STANDARD})
set(CMAKE_CXX_STANDARD 14)
message("chipeak 1 CMAKE_CXX_STANDARD: ")
message(${CMAKE_CXX_STANDARD})


message(" ")
message("chipeak 2 find CUDA start: ")
find_package(CUDA REQUIRED)
if(CUDA_FOUND)
	message("cuda found!!")
	message("CUDA_INCLUDE_DIRS:${CUDA_INCLUDE_DIRS}")
endif(CUDA_FOUND)
message("chipeak 3 find CUDA end!")


set(CMAKE_CUDA_COMPILER /usr/local/cuda/bin/nvcc)
#et(CUDA_NVCC_FLAGS "${CUDA_NVCC_FLAGS};--expt-extended-lambda;-arch=compute_35;-lineinfo;")
set(CUDA_NVCC_FLAGS "${CUDA_NVCC_FLAGS};--expt-extended-lambda;")


message(" ")
message("chipeak 4 enable_language(CUDA) start")
enable_language(CUDA)
message("chipeak 5 enable_language(CUDA) end")

#不管使用哪一种模式，只要找到包，就会定义下面这些变量：
#<NAME>_FOUND
#<NAME>_INCLUDE_DIRS or <NAME>_INCLUDES
#<NAME>_LIBRARIES or <NAME>_LIBRARIES or <NAME>_LIBS
#<NAME>_DEFINITIONS
message(" ")
message("chipeak 6 find_package(OpenCV REQUIRED) start")
find_package(OpenCV REQUIRED)
if(OpenCV_FOUND)
  message("INCLUDE_DIRS:${OpenCV_INCLUDE_DIRS}")
  message("LIBRARIES:${OpenCV_LIBRARIES}")
  message("DEFINITIONS:${OpenCV_DEFINITIONS}")
endif(OpenCV_FOUND)  
message("chipeak 7 find_package(OpenCV REQUIRED) end")

# TensorRT 头文件/动态库
message(" ")
message("TensorRT 头文件&动态库加载:start")
# 相当于gcc -I xxx，向工程添加特定的头文件搜索路径
include_directories("/usr/include/aarch64-linux-gnu")
# 相当于gcc -L xxx，添加非标准的共享库搜索路径
link_directories("/usr/lib/aarch64-linux-gnu")
message("TensorRT 头文件&动态库加载:end")

#include_directories("/usr/local/cuda-10.0/targets/aarch64-linux/include/thrust/system/cuda/detail")
#include_directories("/media/nvidia/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/thrust/thrust/system/cuda/detail")

include_directories("/media/zjx/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/cub")
#include_directories("/usr/local/cuda-10.2/targets/aarch64-linux/include/thrust/system/cuda/detail")    
include_directories("/usr/local/cuda-10.2/targets/aarch64-linux/include")
include_directories("/usr/include/opencv4")

#cmake -DCMAKE_CXX_FLAGS="-std=c++14" -DCMAKE_CUDA_FLAGS="--expt-extended-lambda -std=c++14" ..


#SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14")
#set(CUDA_NVCC_FLAGS -O3; -G; -g;-std=c++14)
#SET(CUDA_NVCC_FLAGS ${CUDA_NVCC_FLAGS};-gencode arch=compute_61,code=sm_61;-std=c++11;)

#cmake -DCMAKE_CUDA_FLAGS="--expt-extended-lambda -std=c++14" ..
set(CMAKE_CUDA_FLAGS "--expt-extended-lambda -std=c++14")

message(" ")
message("add_library retinanet")
# 生成动态库
add_library(retinanet SHARED
  ./csrc/cuda/decode.h
  ./csrc/cuda/decode.cu
  ./csrc/cuda/nms.h
  ./csrc/cuda/nms.cu
  ./csrc/cuda/decode_rotate.h
  ./csrc/cuda/decode_rotate.cu
  ./csrc/cuda/nms_iou.h
  ./csrc/cuda/nms_iou.cu
  ./csrc/cuda/utils.h
  ./csrc/engine.h
  ./csrc/engine.cpp
  ./csrc/calibrator.h
)

# https://cmake.org/cmake/help/latest/prop_tgt/CUDA_RESOLVE_DEVICE_SYMBOLS.html
# 3.9版的新功能
# 仅限CUDA：在需要时为特定的库目标启用设备链接
set_target_properties(retinanet PROPERTIES
  CUDA_RESOLVE_DEVICE_SYMBOLS ON
)

message(" ")
message("chipeak 8 target_link_libraries begin")
include_directories(${OpenCV_INCLUDE_DIRS})
target_link_libraries(retinanet PUBLIC nvinfer nvonnxparser ${OpenCV_LIBS})
message("chipeak 9 target_link_libraries end")

message(" ")
message("chipeak 10 add_executable export begin")
add_executable(export export.cpp)
include_directories(${OpenCV_INCLUDE_DIRS})
target_link_libraries(export PRIVATE retinanet ${OpenCV_LIBS})
message("chipeak 11 add_executable export end")

message(" ")
message("chipeak 12 add_executable infer begin")
add_executable(infer infer.cpp)
include_directories(${OpenCV_INCLUDE_DIRS})
target_link_libraries(infer PRIVATE retinanet ${OpenCV_LIBS} cuda ${CUDA_LIBRARIES})
message("chipeak 13 add_executable infer end")

message(" ")
message("chipeak 14 add_executable infer begin")
message("CMAKE_SYSTEM_PROCESSOR:${CMAKE_SYSTEM_PROCESSOR}")
if(CMAKE_SYSTEM_PROCESSOR MATCHES "aarch64")
  message("hello ...")
  add_executable(infervideo infervideo.cpp)
  include_directories(${OpenCV_INCLUDE_DIRS})
  target_link_libraries(infervideo PRIVATE retinanet ${OpenCV_LIBS} cuda ${CUDA_LIBRARIES})
endif()
message("chipeak 15 add_executable infer end")
message(" ")



