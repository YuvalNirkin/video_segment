cmake_minimum_required(VERSION 2.8)
project(video_segment)

# Set version
# ===================================================
set(VIDEO_SEGMENT_MAJOR_VERSION 1)
set(VIDEO_SEGMENT_MINOR_VERSION 0)
set(VIDEO_SEGMENT_VERSION ${VIDEO_SEGMENT_MAJOR_VERSION}.${VIDEO_SEGMENT_MINOR_VERSION})

# Global configurations
# ===================================================
if(UNIX)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14")
  set(DEF_INSTALL_CMAKE_DIR lib/cmake/${PROJECT_NAME})
else()
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DNOMINMAX")
  set(DEF_INSTALL_CMAKE_DIR cmake)
endif() 
set(INSTALL_CMAKE_DIR ${DEF_INSTALL_CMAKE_DIR} CACHE PATH
	"Installation directory for CMake files")
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake")
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

cmake_policy(SET CMP0020 NEW)

# Optional 3rd party components
# ===================================================
option(WITH_BOOST_STATIC "Boost static libraries" ON)

# Find dependencies
# ===================================================
# Boost
set(Boost_USE_STATIC_LIBS ${WITH_BOOST_STATIC})
set(BOOST_ALL_DYN_LINK NOT ${WITH_BOOST_STATIC})
if(WIN32)
	if(${WITH_BOOST_STATIC})
		add_definitions(-DBOOST_ALL_NO_LIB)
	else()
		add_definitions(-DBOOST_ALL_DYN_LINK)
	endif()
endif()
find_package(Boost REQUIRED filesystem program_options date_time thread system chrono)

find_package( OpenCV REQUIRED highgui imgproc imgcodecs video videoio)

find_package(glog REQUIRED)

find_package(protobuf REQUIRED)

find_package(FFMPEG REQUIRED)

find_package(Qt5Widgets REQUIRED)

# Set build output directories
#set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
#set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
#set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Process subdirectories
# ===================================================
add_subdirectory(base)
add_subdirectory(imagefilter)
add_subdirectory(segment_util)
add_subdirectory(video_framework)
add_subdirectory(segmentation)
add_subdirectory(seg_tree_sample)
add_subdirectory(segment_converter)
add_subdirectory(segment_viewer)
add_subdirectory(video_display_qt)

# Export configuration
# ===================================================

# Add all targets to the build-tree export set
export(TARGETS base imagefilter segment_util video_framework segmentation
	seg_tree_sample segment_converter segment_viewer video_display_qt
  FILE "${PROJECT_BINARY_DIR}/video_segment-targets.cmake")
  
# Create the *-config.cmake and *-config-version.cmake files
set(CONF_INCLUDE_DIRS "\${VIDEO_SEGMENT_CMAKE_DIR}/include")
configure_file(cmake/video_segment-config.cmake.in
  "${PROJECT_BINARY_DIR}/video_segment-config.cmake" @ONLY)
configure_file(cmake/video_segment-config-version.cmake.in
  "${PROJECT_BINARY_DIR}/video_segment-config-version.cmake" @ONLY)
   
# Install the video_segment-config.cmake and video_segment-config-version.cmake
install(FILES
  "${PROJECT_BINARY_DIR}/video_segment-config.cmake"
  "${PROJECT_BINARY_DIR}/video_segment-config-version.cmake"
  DESTINATION ${INSTALL_CMAKE_DIR} COMPONENT dev)
 
# Install the export set for use with the install-tree
install(EXPORT video_segment-targets DESTINATION ${INSTALL_CMAKE_DIR} COMPONENT dev)