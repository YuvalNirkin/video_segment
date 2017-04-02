cmake_minimum_required(VERSION 2.6)

project(base)

include("common")
#set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
#include("${CMAKE_MODULE_PATH}/common.cmake")
#include("${CMAKE_SOURCE_DIR}/depend.cmake")

set(SOURCES base.cpp
            base_impl.cpp)
headers_from_sources_cpp(HEADERS "${SOURCES}")

set(SOURCES "${SOURCES}" "${HEADERS}")

add_library(base ${SOURCES})

target_include_directories(base PRIVATE ${CMAKE_SOURCE_DIR})
target_include_directories(base PUBLIC ${Boost_INCLUDE_DIRS} ${glog_INCLUDE_DIRS})
target_link_libraries(base PUBLIC ${Boost_LIBRARIES} ${glog_LIBRARIES})
set_target_properties(base PROPERTIES DEBUG_POSTFIX _d)

# Installations
install(TARGETS base
	 EXPORT video_segment-targets
     RUNTIME DESTINATION bin COMPONENT dev
     LIBRARY DESTINATION lib COMPONENT dev
     ARCHIVE DESTINATION lib COMPONENT dev)
install(FILES ${HEADERS} DESTINATION include/base)