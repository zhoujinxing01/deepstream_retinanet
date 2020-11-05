cmake_minimum_required(VERSION 3.5.1)

project(deepstream-retinanet)
enable_language(CXX)
include(FindCUDA)

set(CMAKE_CXX_STANDARD 14)
find_package(CUDA REQUIRED)
find_package(OpenCV REQUIRED)

if(DEFINED TensorRT_DIR)
  include_directories("${TensorRT_DIR}/include")
  link_directories("${TensorRT_DIR}/lib")
endif(DEFINED TensorRT_DIR)
if(DEFINED DeepStream_DIR)
  include_directories("${DeepStream_DIR}/sources/includes")
endif(DEFINED DeepStream_DIR)
include_directories(${CUDA_INCLUDE_DIRS})

if(NOT DEFINED ARCH)
  message("ARCH:ChiPeak")
  #set(ARCH "sm_70")
  set(ARCH "sm_62")
endif(NOT DEFINED ARCH)


include_directories("/media/zjx/76524d80-8abc-4b9c-a262-a82f0f7c0b43/cuda/cub")
include_directories("/opt/nvidia/deepstream/deepstream-5.0/sources/includes")


cuda_add_library(nvdsparsebbox_retinanet SHARED
  ../../cppai/csrc/cuda/decode.h
  ../../cppai/csrc/cuda/decode.cu
  ../../cppai/csrc/cuda/nms.h
  ../../cppai/csrc/cuda/nms.cu
  ../../cppai/csrc/cuda/utils.h
  ../../cppai/csrc/engine.cpp
  nvdsparsebbox_retinanet.cpp
  OPTIONS -arch ${ARCH} -std=c++14 --expt-extended-lambda
)
include_directories(${OpenCV_INCLUDE_DIRS})
target_link_libraries(nvdsparsebbox_retinanet ${CUDA_LIBRARIES} nvinfer nvinfer_plugin nvonnxparser ${OpenCV_LIBS})
