cmake_minimum_required(VERSION 2.8)

project(OpenNI)

if(UNIX AND CMAKE_SYSTEM_NAME MATCHES Linux)
    set(LINUX ON)
endif()

if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(M64 ON)
else()
    set(M32 ON)
endif()

if(NOT DEFINED MANCTL_CHANGES)
    set(       MANCTL_CHANGES ON)
endif()

# Options
# ---------
add_definitions(-DCUSTOM_SAMPLE_XML_PATH="config/SamplesConfig.xml")
add_definitions(-DCUSTOM_SAMPLE_USER_XML_PATH="config/Sample-User.xml")
if(MANCTL_CHANGES)
add_definitions(-DXN_USE_CUSTOM_MODULES_FILE=xn_modules_file)
else()
add_definitions(-DXN_USE_CUSTOM_MODULES_FILE="config/modules.xml")
endif()

ADD_DEFINITIONS(-DUSE_GLUT)

# Find platform
# -------------
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
	SET(OPENNI_PLATFORM_LINUX true)
elseif (WIN32)
	set(OPENNI_PLATFORM_WIN32 true)
elseif (APPLE)
	set(OPENNI_PLATFORM_APPLE true)
endif()

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake" ${CMAKE_MODULE_PATH})
set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/lib)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_INSTALL_RPATH ../lib)

# Some global variables
# ----------------------
set(TINYXML_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/Externals/TinyXml/")
set(OPENNI_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/Include/")
set(OPENNI_MODULES_DIR "${CMAKE_CURRENT_SOURCE_DIR}/Source/Modules")

INCLUDE_DIRECTORIES(${OPENNI_INCLUDE_DIR})

#IF (APPLE)
#  LINK_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/Nite/Lib/MacOSX)
#ELSEIF (UNIX)
#  IF(ARCHITECTURE_IS_X86_64)
#    LINK_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/Nite/Lib/Linux64)
#    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC" )
#  ELSE()
#    LINK_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/Nite/Lib/Linux32)
#  ENDIF()
#ENDIF()

# Find JPEG system lib
# ---------------------
find_package(jpeg REQUIRED NO_MODULE)
#if(NOT JPEG_FOUND)
#	# when not found use the one in the externals folder, so we can compile!
#	set(JPEG_FOUND true)
#	set(JPEG_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/Source/External/LibJPEG/")
#	set(JPEG_LIBRARY jpeg)
#	add_subdirectory("./Source/External/LibJPEG/")
#endif()

# Find USB lib
# -------------

find_package(usb REQUIRED)

# @todo find tinyxml!

# Use external libraries.
# -----------------------
add_subdirectory("Externals/TinyXml/") # @todo only when lib was not found
add_subdirectory("Source/OpenNI/")
add_subdirectory("Source/Modules/")

# Add samples
# ---------------------------
if(OPENNI_BUILD_SAMPLES)
    add_subdirectory(Samples)
endif()

# Utils
# ---------------------------
add_subdirectory("./Source/Utils/")

#add_subdirectory(SensorKinect)

#SET(NITE_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/Nite/Include)
#add_subdirectory(Nite)

IF(NOT IS_DIRECTORY ${EXECUTABLE_OUTPUT_PATH}/config)
  file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/config DESTINATION ${EXECUTABLE_OUTPUT_PATH}/)
ENDIF()

macro(link_modules_xml suffix)
    execute_process(
        COMMAND ${CMAKE_COMMAND} -E create_symlink modules.xml.${suffix} modules.xml
        WORKING_DIRECTORY ${EXECUTABLE_OUTPUT_PATH}/config
    )
endmacro()

if(APPLE)
    link_modules_xml(mac)
elseif(UNIX)
    link_modules_xml(linux)
elseif(WIN32)
    link_modules_xml(win32)
endif()

foreach(dir . MacOSX Linux-x86 Linux-Arm Win32)
  file(GLOB HEADERS "Include/${dir}/*.h")
  INSTALL(FILES ${HEADERS} DESTINATION include/ni/${dir})
endforeach()

install(DIRECTORY ${EXECUTABLE_OUTPUT_PATH}/config DESTINATION bin/)

install(FILES  openni-config.cmake DESTINATION share/openni)
install(EXPORT openni-exports      DESTINATION share/openni)
