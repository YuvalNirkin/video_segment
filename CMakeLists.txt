cmake_minimum_required(VERSION 2.8)
project(video_segment)

# Set version
# ===================================================
set(VIDEO_SEGMENT_MAJOR_VERSION 1)
set(VIDEO_SEGMENT_MINOR_VERSION 0)
set(VIDEO_SEGMENT_VERSION ${VIDEO_SEGMENT_MAJOR_VERSION}.${VIDEO_SEGMENT_MINOR_VERSION})

# Global configurations
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake")

if(UNIX)
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
else()
	 SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DNOMINMAX")
endif() 

# Optional build components
# ===================================================

# Find dependencies
# ===================================================
# Boost
if(MSVC)
	set(Boost_USE_STATIC_LIBS ON)	# only find static libs
endif(MSVC)
set(Boost_USE_MULTITHREADED ON)
find_package( Boost REQUIRED filesystem regex program_options)
if(Boost_FOUND)
	include_directories(${Boost_INCLUDE_DIRS})
	if(MSVC)
		# On Windows the Boost libraries are automatically linked
		link_directories(${Boost_LIBRARY_DIRS})
	endif(MSVC)	
endif()

find_package( OpenCV REQUIRED highgui imgproc imgcodecs video videoio)

find_package(glog REQUIRED)

find_package(protobuf REQUIRED)

find_package(FFMPEG REQUIRED)

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

# Export configuration
# ===================================================

# Add all targets to the build-tree export set
export(TARGETS base imagefilter segment_util video_framework segmentation
	seg_tree_sample segment_converter segment_viewer
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
  DESTINATION "cmake" COMPONENT dev)
 
# Install the export set for use with the install-tree
install(EXPORT video_segment-targets DESTINATION cmake COMPONENT dev)