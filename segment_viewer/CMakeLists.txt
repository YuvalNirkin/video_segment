cmake_minimum_required(VERSION 2.6)
project(segment_viewer)

#set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/../cmake")
include(${CMAKE_MODULE_PATH}/common.cmake)
#include("${CMAKE_SOURCE_DIR}/depend.cmake")

add_executable(segment_viewer viewer.cpp)
target_include_directories(segment_viewer PRIVATE ${CMAKE_SOURCE_DIR} ${CMAKE_BINARY_DIR})
target_link_libraries(segment_viewer PRIVATE base segment_util)
set_target_properties(segment_viewer PROPERTIES DEBUG_POSTFIX _d)
#apply_dependencies(segment_viewer)

# Installations
install(TARGETS segment_viewer
	 EXPORT video_segment-targets
     RUNTIME DESTINATION bin COMPONENT dev
     LIBRARY DESTINATION lib COMPONENT dev
     ARCHIVE DESTINATION lib COMPONENT dev)