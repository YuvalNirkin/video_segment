cmake_minimum_required(VERSION 2.6)

project(segment_converter)

include("common")
#set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/../cmake")
#include(${CMAKE_MODULE_PATH}/common.cmake)
#include("${CMAKE_SOURCE_DIR}/depend.cmake")

add_executable(segment_converter converter.cpp)
target_link_libraries(segment_converter PRIVATE base segment_util)
target_include_directories(segment_converter PRIVATE ${CMAKE_SOURCE_DIR} ${CMAKE_BINARY_DIR})
target_include_directories(segment_converter PRIVATE ${gflags_INCLUDE_DIRS})
target_link_libraries(segment_converter PRIVATE ${gflags_LIBRARIES})
set_target_properties(segment_converter PROPERTIES DEBUG_POSTFIX _d)

#apply_dependencies(segment_converter)

# Installations
install(TARGETS segment_converter
	 EXPORT video_segment-targets
     RUNTIME DESTINATION bin COMPONENT dev
     LIBRARY DESTINATION lib COMPONENT dev
     ARCHIVE DESTINATION lib COMPONENT dev)