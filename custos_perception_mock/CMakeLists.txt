cmake_minimum_required(VERSION 3.16)
project(custos_perception_mock)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 20)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_lifecycle REQUIRED)
find_package(lifecycle_msgs REQUIRED)
find_package(diagnostic_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(custos_common_msgs REQUIRED)
find_package(custos_perception_msgs REQUIRED)

# Mock node source — initially empty; satisfies the same lifecycle, topics,
# services, and diagnostics as custos-novatek-sdk-wrapper per ADR 0010.
# add_executable(perception_mock_node src/perception_mock_node.cpp)
# ament_target_dependencies(perception_mock_node
#   rclcpp rclcpp_lifecycle lifecycle_msgs diagnostic_msgs sensor_msgs
#   custos_common_msgs custos_perception_msgs
# )
# install(TARGETS perception_mock_node DESTINATION lib/${PROJECT_NAME})

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
