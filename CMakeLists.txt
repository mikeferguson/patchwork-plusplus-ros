cmake_minimum_required(VERSION 3.5)
project(patchworkpp)
add_compile_options(-std=c++17)
set(CMAKE_BUILD_TYPE "Release")

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_definitions(-DTARGET_ROS2)
find_package(ament_cmake REQUIRED)
find_package(pcl_ros REQUIRED)
find_package(pcl_conversions REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_components REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(Boost 1.54 REQUIRED)

include_directories(include)

set(DEPENDENCIES
  pcl_ros
  pcl_conversions
  rclcpp
  rclcpp_components
  sensor_msgs
  std_msgs
  tf2_ros
)

add_library(patchworkpp SHARED src/patchworkpp.cpp)
target_compile_definitions(patchworkpp PUBLIC "PATCHWORKPP_COMPOSITION_BUILDING_DLL")
ament_target_dependencies(patchworkpp ${DEPENDENCIES})

add_executable(demo src/demo.cpp)
ament_target_dependencies(demo ${DEPENDENCIES})
target_link_libraries(demo ${PCL_LIBRARIES})

install(
  TARGETS
    demo
    patchworkpp
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

install(
  DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include/${PROJECT_NAME}
)

install(
  DIRECTORY launch
  DESTINATION share/${PROJECT_NAME}
)

ament_export_dependencies(ament_cmake)
ament_export_dependencies(${DEPENDENCIES})
ament_export_include_directories(${INCLUDE_DIRS})
ament_package()
