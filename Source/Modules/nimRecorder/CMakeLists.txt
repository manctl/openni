file(GLOB nimRecorder_sources ./*.cpp)

include_directories(${OPENNI_INCLUDE_DIR})
include_directories(${OPENNI_MODULES_DIR}/Common/)


add_library(nimRecorder SHARED ${nimRecorder_sources})
target_link_libraries(nimRecorder OpenNI nimCommon)


install(TARGETS nimRecorder EXPORT openni-exports
	ARCHIVE DESTINATION lib
	RUNTIME DESTINATION bin
)
