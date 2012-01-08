file(GLOB nimCodecs_sources ./*.cpp)

include_directories(${JPEG_INCLUDE_DIR})
include_directories(${OPENNI_INCLUDE_DIR})
include_directories(${OPENNI_MODULES_DIR}/Common/)

add_library(nimCodecs SHARED ${nimCodecs_sources})
target_link_libraries(nimCodecs nimCommon jpeg OpenNI)

install(TARGETS nimCodecs EXPORT openni-exports
	ARCHIVE DESTINATION lib
	LIBRARY DESTINATION lib
	RUNTIME DESTINATION bin
)
