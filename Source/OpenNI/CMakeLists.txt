# Add search paths.
# ------------------
include_directories(${TINYXML_INCLUDE_DIR})
include_directories(${OPENNI_INCLUDE_DIR})
include_directories("./FilesOnly")

# Get sources for OpenNI
# ----------------------
file(GLOB OpenNI_sources ./*.cpp ./*.h ../../Include/*.h)

# Add platform specific sources.
# ------------------------------
if(OPENNI_PLATFORM_LINUX)
	file(GLOB OpenNI_platform_sources ./Linux-x86/*.cpp)
elseif(OPENNI_PLATFORM_APPLE)
	file(GLOB OpenNI_platform_sources ./Linux-x86/*.cpp)
	set(CMAKE_EXE_LINKER_FLAGS "-framework CoreFoundation -framework IOKit")
endif()

# Set our target
# ---------------
add_library(OpenNI SHARED 
	${OpenNI_sources} 
	${OpenNI_platform_sources}
)

set_property(TARGET OpenNI PROPERTY LINK_INTERFACE_LIBRARIES "")

# Set Link libraries.
# -------------------
find_library(LIBDL dl)
target_link_libraries(OpenNI
        usb
	${LIBDL} 
	TinyXml
#	${CMAKE_EXE_LINKER_FLAGS}
)

# Set path where to install the target, lib, runtime. 
# (to same dir where we call cmake ., as we use the "dot")
# -----------------------------------------------------
install(TARGETS OpenNI EXPORT openni-exports
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)
