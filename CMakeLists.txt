cmake_minimum_required(VERSION 2.8)
project(vo1)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_STANDARD 14)

add_definitions("-DENABLE_SSE")
set(CMAKE_CXX_FLAGS "-std=c++11 -O2 ${SSE_FLAGS} -msse4")
list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

find_package(OpenCV 3 REQUIRED)
find_package(Sophus REQUIRED)
find_package(G2O REQUIRED)

include_directories(
			${OpenCV_INCLUDE_DIRS} 
			${G2O_INCLUDE_DIRS}
			${Sophus_INCLUDE_DIRS} 
			"/usr/include/eigen3/")


add_executable(orb_cv orb_cv.cpp)
target_link_libraries(orb_cv ${OpenCV_LIBS})

add_executable(pose_estimation2d2d pose_estimation_2d2d.cpp)
target_link_libraries(pose_estimation2d2d ${OpenCV_LIBS})

add_executable(tri triangulation.cpp)
target_link_libraries(tri ${OpenCV_LIBS})

add_executable(pose_estimation_3d2d pose_estimation_3d2d.cpp)
target_link_libraries(pose_estimation_3d2d 
				${OpenCV_LIBS} 
				Sophus::Sophus 
				g2o_core g2o_stuff)

add_executable(pose_estimation_3d3d pose_estimation_3d3d.cpp)
target_link_libraries(pose_estimation_3d3d g2o_core g2o_stuff ${OpenCV_LIBS} Sophus::Sophus)