cmake_minimum_required(VERSION 3.5)
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
find_package(tf2_ros REQUIRED)

find_package(Boost REQUIRED COMPONENTS system thread)

find_package(Eigen REQUIRED)

ament_python_install_package(${PROJECT_NAME})

include_directories(include)

add_library(laser_geometry SHARED src/laser_geometry.cpp)

ament_target_dependencies(laser_geometry
  "rclcpp"
  "sensor_msgs"
  "tf2"
  "tf2_ros")

target_include_directories(laser_geometry
  PUBLIC ${angles_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS} ${Eigen_INCLUDE_DIRS}
)

target_link_libraries(laser_geometry
  ${angles_LIBRARIES}
  ${Boost_LIBRARIES}
  ${Eigen_LIBRARIES}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()

  #ament_add_gtest(projection_test test/projection_test.cpp)
  #target_link_libraries(projection_test laser_geometry ${LIBS})

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
ament_export_dependencies(tf2_ros)
ament_export_libraries(laser_geometry)

ament_package()
