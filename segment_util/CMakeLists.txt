cmake_minimum_required(VERSION 2.6)

project(segment_util)
#set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/../cmake")
include(${CMAKE_MODULE_PATH}/common.cmake)
#include("${CMAKE_SOURCE_DIR}/depend.cmake")

set(SOURCES segmentation_io.cpp
            segmentation_util.cpp
            segmentation_render.cpp
            segmentation_boundary.cpp
            )

headers_from_sources_cpp(HEADERS "${SOURCES}")
set(SOURCES "${SOURCES}" "${HEADERS}")

# Custom build steps.
find_package(Protobuf REQUIRED)
include_directories(${CMAKE_CURRENT_BINARY_DIR})
protobuf_generate_cpp(PROTO_SRC PROTO_HDR segmentation.proto)

add_library(segment_util ${SOURCES} ${PROTO_SRC} ${PROTO_HDR})

target_include_directories(segment_util PRIVATE ${CMAKE_SOURCE_DIR} ${CMAKE_BINARY_DIR})
target_include_directories(segment_util PUBLIC ${Boost_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS}
	${glog_INCLUDE_DIRS} ${PROTOBUF_INCLUDE_DIRS})
target_link_libraries(segment_util PUBLIC ${Boost_LIBRARIES} ${OpenCV_LIBRARIES} ${glog_LIBRARIES} ${PROTOBUF_LIBRARIES})
set_target_properties(segment_util PROPERTIES DEBUG_POSTFIX _d)

#apply_dependencies(segment_util)

# Installations
install(TARGETS segment_util
	 EXPORT video_segment-targets
     RUNTIME DESTINATION bin COMPONENT dev
     LIBRARY DESTINATION lib COMPONENT dev
     ARCHIVE DESTINATION lib COMPONENT dev)
install(FILES ${HEADERS} DESTINATION include/segment_util)
install(FILES ${PROTO_HDR} DESTINATION include/segment_util)