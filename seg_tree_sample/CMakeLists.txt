cmake_minimum_required(VERSION 2.6)

project(seg_tree_sample)

#set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/../cmake")
include(${CMAKE_MODULE_PATH}/common.cmake)
#include("${CMAKE_SOURCE_DIR}/depend.cmake")

set(CMAKE_INCLUDE_CURRENT_DIR 1)
option(NO_X_SUPPORT "Disables interactive output and linking of X libraries" OFF)
option(WITH_QT "Enables QT video and segmentation display" OFF)

if (NO_X_SUPPORT)
  add_definitions(-DNO_X_SUPPORT)
endif(NO_X_SUPPORT)

if (WITH_QT)
  add_definitions(-DWITH_QT)
endif(WITH_QT)

add_executable(seg_tree_sample seg_tree.cpp)
target_include_directories(seg_tree_sample PRIVATE ${CMAKE_SOURCE_DIR} ${CMAKE_BINARY_DIR})
target_link_libraries(seg_tree_sample PRIVATE base video_framework segmentation)
if (WITH_QT)
target_link_libraries(seg_tree_sample PRIVATE video_display_qt)
endif(WITH_QT)
if(UNIX)
	target_link_libraries(vsal PRIVATE ${Boost_LIBRARIES})
endif(UNIX)
set_target_properties(seg_tree_sample PROPERTIES DEBUG_POSTFIX _d)

#apply_dependencies(seg_tree_sample)

# Installations
install(TARGETS seg_tree_sample
	 EXPORT video_segment-targets
     RUNTIME DESTINATION bin COMPONENT dev
     LIBRARY DESTINATION lib COMPONENT dev
     ARCHIVE DESTINATION lib COMPONENT dev)