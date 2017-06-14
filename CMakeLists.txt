cmake_minimum_required(VERSION 2.8.3)
project(laser_geometry)

# Add support for C++11
if(NOT WIN32)
  add_definitions(-std=c++11)
endif()

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)
find_package(angles REQUIRED)
find_package(cmake_modules REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(tf2 REQUIRED)
find_package(ros2_time REQUIRED)
find_package(ros2_console REQUIRED)

find_package(Boost REQUIRED COMPONENTS system thread)

find_package(Eigen REQUIRED)

ament_python_install_package(${PROJECT_NAME})

set(INCLUDE_DIRS include ${ament_cmake_INCLUDE_DIRS} ${angles_INCLUDE_DIRS}
  ${cmake_modules_INCLUDE_DIRS} ${rclcpp_INCLUDE_DIRS} ${sensor_msgs_INCLUDE_DIRS}
  ${tf2_INCLUDE_DIRS} ${ros2_time_INCLUDE_DIRS} ${ros2_console_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS} ${Eigen_INCLUDE_DIRS})
include_directories(${INCLUDE_DIRS})

set(LIBRARY_DIRS ${ament_cmake_LIBRARY_DIRS} ${angles_LIBRARY_DIRS}
  ${cmake_modules_LIBRARY_DIRS} ${rclcpp_LIBRARY_DIRS} ${sensor_msgs_LIBRARY_DIRS}
  ${tf2_LIBRARY_DIRS} ${ros2_time_LIBRARY_DIRS} ${ros2_console_LIBRARY_DIRS}
  ${Boost_LIBRARY_DIRS} ${Eigen_LIBRARY_DIRS})

link_directories(${LIBRARY_DIRS})

set(LIBS ${ament_cmake_LIBRARIES} ${angles_LIBRARIES} ${cmake_modules_LIBRARIES}
  ${rclcpp_LIBRARIES} ${sensor_msgs_LIBRARIES} ${tf2_LIBRARIES} ${ros2_time_LIBRARIES}
  ${ros2_console_LIBRARIES} ${Boost_LIBRARIES} ${Eigen_LIBRARIES})

add_library(laser_geometry src/laser_geometry.cpp)
target_link_libraries(laser_geometry ${LIBS})

if(BUILD_TESTING)
  ament_add_gtest(projection_test test/projection_test.cpp)
  target_link_libraries(projection_test laser_geometry ${LIBS})

  ament_add_nose_test(${PROJECT_NAME}_projection_nose_test test/projection_test.py)
endif()

install(TARGETS laser_geometry
  ARCHIVE
  DESTINATION lib
  LIBRARY
  DESTINATION lib)
install(DIRECTORY include/laser_geometry/
  DESTINATION include/${PROJECT_NAME}
  FILES_MATCHING
  PATTERN "*.h")

ament_export_dependencies(ament_cmake)
ament_export_dependencies(angles)
ament_export_dependencies(cmake_modules)
ament_export_dependencies(rclcpp)
ament_export_dependencies(sensor_msgs)
ament_export_dependencies(tf2)
ament_export_dependencies(ros2_time)
ament_export_dependencies(ros2_console)
ament_export_include_directories(${INCLUDE_DIRS})
ament_export_libraries(laser_geometry ${LIBS})

ament_package()
