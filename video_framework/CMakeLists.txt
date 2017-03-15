cmake_minimum_required(VERSION 2.6)

project(video_framework)

#set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/../cmake")
include(${CMAKE_MODULE_PATH}/common.cmake)
#include("${CMAKE_SOURCE_DIR}/depend.cmake")

set(SOURCES video_unit.cpp
            video_display_unit.cpp
            video_reader_unit.cpp
            video_writer_unit.cpp
            conversion_units.cpp
            flow_reader.cpp
            video_capture_unit.cpp
            video_pipeline.cpp
         )

headers_from_sources_cpp(HEADERS "${SOURCES}")
set(HEADERS ${HEADERS} concurrent_queue.h)
set(SOURCES "${SOURCES}" "${HEADERS}" )

add_library(video_framework ${SOURCES})

target_include_directories(video_framework PRIVATE ${CMAKE_SOURCE_DIR} ${CMAKE_BINARY_DIR})
target_include_directories(video_framework PUBLIC ${Boost_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS} ${glog_INCLUDE_DIRS}
	${PROTOBUF_INCLUDE_DIRS} ${FFMPEG_INCLUDE_DIRS})
target_link_libraries(video_framework PUBLIC ${Boost_LIBRARIES} ${OpenCV_LIBRARIES} ${glog_LIBRARIES}
	${PROTOBUF_LIBRARIES} ${FFMPEG_LIBRARIES})
set_target_properties(video_framework PROPERTIES DEBUG_POSTFIX _d)

#apply_dependencies(video_framework)

# Installations
install(TARGETS video_framework
	 EXPORT video_segment-targets
     RUNTIME DESTINATION bin COMPONENT dev
     LIBRARY DESTINATION lib COMPONENT dev
     ARCHIVE DESTINATION lib COMPONENT dev)
install(FILES ${HEADERS} DESTINATION include/video_framework)