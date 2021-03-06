# Copyright 2018 Apex.AI, Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# Co-developed by Tier IV, Inc. and Apex.AI, Inc.
cmake_minimum_required(VERSION 3.5)

project(serial_driver)

## dependencies
find_package(ament_cmake_auto REQUIRED)
find_package(Boost REQUIRED COMPONENTS system)

ament_auto_find_build_dependencies()

include_directories(include ${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

if(BUILD_TESTING)
  find_package(ament_lint_auto)
  ament_lint_auto_find_test_dependencies()

  set(TEST_LIB "${PROJECT_NAME}_test")
  add_library(${TEST_LIB} SHARED test/test_driver.hpp test/test_driver.cpp)
  ament_target_dependencies(${TEST_LIB} "rclcpp" "std_msgs")
  target_include_directories(${TEST_LIB} PRIVATE include)
  target_link_libraries(${TEST_LIB} ${Boost_LIBRARIES})

  # Unit tests
  set(TEST_SERIAL_DRIVER_EXE test_serial_driver)
  ament_add_gtest(${TEST_SERIAL_DRIVER_EXE}
    test/gtest_main.cpp
    test/test_serial_driver.cpp)
  ament_target_dependencies(${TEST_SERIAL_DRIVER_EXE}  "Boost")
  target_include_directories(${TEST_SERIAL_DRIVER_EXE} PRIVATE include)
  target_link_libraries(${TEST_SERIAL_DRIVER_EXE} ${TEST_LIB} util)
endif()

ament_auto_package(
  CONFIG_EXTRAS_POST "serial_driver-extras.cmake"
)
