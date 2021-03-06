cmake_minimum_required(VERSION 3.1.0)

project(video_display_qt)

include("common")
#set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/../cmake")
#include(${CMAKE_MODULE_PATH}/common.cmake)
#include("${CMAKE_SOURCE_DIR}/depend.cmake")

#find_package(Qt4 COMPONENTS QtGui QtCore REQUIRED)

qt5_wrap_cpp(MOC_SOURCES main_window.h)

set(SOURCES video_display_qt_unit.cpp
	          main_window.cpp
            )

headers_from_sources_cpp(HEADERS "${SOURCES}")
set(SOURCES "${SOURCES}"
            "${HEADERS}"
             ${MOC_SOURCES}
)

add_library(video_display_qt ${SOURCES})

target_include_directories(video_display_qt PRIVATE ${CMAKE_SOURCE_DIR} ${CMAKE_BINARY_DIR})
target_link_libraries(video_display_qt PRIVATE base video_framework segment_util)
target_link_libraries(video_display_qt PUBLIC Qt5::Widgets)
if(UNIX)
	target_link_libraries(video_display_qt PUBLIC ${Boost_LIBRARIES})
endif(UNIX)
set_target_properties(video_display_qt PROPERTIES DEBUG_POSTFIX _d)

#apply_dependencies(video_display_qt)

# Installations
install(TARGETS video_display_qt
	 EXPORT video_segment-targets
     RUNTIME DESTINATION bin COMPONENT dev
     LIBRARY DESTINATION lib COMPONENT dev
     ARCHIVE DESTINATION lib COMPONENT dev)
install(FILES ${HEADERS} DESTINATION include/video_display_qt)