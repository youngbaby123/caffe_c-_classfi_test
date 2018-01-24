cmake_minimum_required(VERSION 2.8)

#SET(CMAKE_BUILD_TYPE "Debug")

project(recognizer)

SET(CMAKE_C_COMPILER g++)
add_definitions(-std=c++11)

file(GLOB_RECURSE main_cpp "src/classification_my.cpp")
#file(GLOB_RECURSE recognizercpp "src/recognizer.cpp")
#file(GLOB_RECURSE utilscpp "src/utils.cpp")
#file(GLOB_RECURSE libjsoncpp "lib_json/libjsoncpp.a")
#file(GLOB_RECURSE jsonhpp "json/json.hpp")
#file(GLOB_RECURSE recognizerhpp "include/recognizer.hpp")
file(GLOB_RECURSE libgflags "/usr/lib/x86_64-linux-gnu/libgflags.so.2")


FIND_PACKAGE(CUDA REQUIRED)

set(Caffe_DIR /home/zkyang/Workspace/task/caffe_task/caffe_clssfi)
#FIND_LIBRARY(CAFFE_LIB caffe path ${Caffe_DIR}/build/lib)
#set(CAFFE_LIB /home/zyb/cv/WL_ssd/build/lib/libcaffe.so)
set(CAFFE_LIB /home/zkyang/Workspace/task/caffe_task/caffe_clssfi/lib/libcaffe.so)

set(GNU_DIR /usr/lib/x86_64-linux-gnu)
set(OpenCV_LIBS /usr/local/lib/libopencv_imgproc.so /usr/local/lib/libopencv_core.so /usr/local/lib/libopencv_highgui.so)

FIND_LIBRARY(GLOG_LIB glog path ${GNU_DIR})
FIND_LIBRARY(BOOST_LIB boost_system path ${GNU_DIR})
option (USE_OPENCV "Build with OpenCV support" ON)
add_definitions(-DUSE_OPENCV)

#set(REC_SOURCE_FILES ${computation_team_srcs} ${recognizerhpp} ${recognizercpp} ${utilscpp})
set(REC_SOURCE_FILES ${main_cpp})

set(PROJECT_ROOT ./)
include_directories(
${PROJECT_SOURCE_DIR}
${CUDA_TOOLKIT_INCLUDE}
${Caffe_DIR}/include
${PROJECT_ROOT}
${OpenCV_INCLUDE_DIRS}
#${PYTHON_INCLUDE_DIRS}
)

add_executable(main ${REC_SOURCE_FILES})

target_link_libraries(main
${libgflags}
${CAFFE_LIB}
${GLOG_LIB}
${BOOST_LIB}
${OpenCV_LIBS}
)
