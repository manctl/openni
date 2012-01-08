file(GLOB nimCommon_sources ./*.cpp)

include_directories(${OPENNI_INCLUDE_DIR})
include_directories(${OPENNI_MODULES_DIR}/Common/)

add_library(nimCommon ${nimCommon_sources})
target_link_libraries(nimCommon OpenNI)

install(TARGETS nimCommon EXPORT openni-exports
	ARCHIVE DESTINATION lib
)
