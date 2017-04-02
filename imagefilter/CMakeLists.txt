cmake_minimum_required(VERSION 2.6)

project(imagefilter)

include("common")
#set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/../cmake")
#include("${CMAKE_MODULE_PATH}/common.cmake")
#include("${CMAKE_SOURCE_DIR}/depend.cmake")

set(SOURCES image_filter.cpp)

headers_from_sources_cpp(HEADERS "${SOURCES}")
set(SOURCES "${SOURCES}" "${HEADERS}" image_util.h)

add_library(imagefilter ${SOURCES})

target_include_directories(imagefilter PRIVATE ${CMAKE_SOURCE_DIR})
target_include_directories(imagefilter PUBLIC ${Boost_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS} ${glog_INCLUDE_DIRS})
target_link_libraries(imagefilter PUBLIC ${OpenCV_LIBRARIES} ${glog_LIBRARIES} ${Boost_LIBRARIES})
set_target_properties(imagefilter PROPERTIES DEBUG_POSTFIX _d)

#apply_dependencies(imagefilter)

# Installations
install(TARGETS imagefilter
	 EXPORT video_segment-targets
     RUNTIME DESTINATION bin COMPONENT dev
     LIBRARY DESTINATION lib COMPONENT dev
     ARCHIVE DESTINATION lib COMPONENT dev)
install(FILES ${HEADERS} DESTINATION include/imagefilter)