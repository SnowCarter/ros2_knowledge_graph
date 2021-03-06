cmake_minimum_required(VERSION 3.5)
project(ros2_knowledge_graph)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_BUILD_TYPE DEBUG)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(ros2_knowledge_graph_msgs REQUIRED)
find_package(pluginlib REQUIRED)

set(dependencies
    rclcpp
    ros2_knowledge_graph_msgs
    pluginlib
)

include_directories(include)

set(BICAGRAPH_SOURCES
  src/ros2_knowledge_graph/Graph.cpp
  src/ros2_knowledge_graph/GraphNode.cpp
  src/ros2_knowledge_graph/Types.cpp
)

add_library(${PROJECT_NAME} SHARED ${BICAGRAPH_SOURCES})
ament_target_dependencies(${PROJECT_NAME} ${dependencies})

add_executable(graph_terminal src/graph_terminal.cpp)
ament_target_dependencies(graph_terminal ${dependencies})
target_link_libraries(graph_terminal ${PROJECT_NAME})

install(DIRECTORY include/
  DESTINATION include/
)

install(TARGETS
  ${PROJECT_NAME}
  graph_terminal
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()

  find_package(ament_cmake_gtest REQUIRED)
  add_subdirectory(test)
endif()

ament_export_include_directories(include)
ament_export_libraries(${PROJECT_NAME})
ament_export_dependencies(${dependencies})

ament_package()
