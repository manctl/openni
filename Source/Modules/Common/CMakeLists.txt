if(LINUX AND M64)
    list(APPEND nimCommon_COMPILE_FLAGS "-fPIC")
endif()

file(GLOB nimCommon_SOURCES ./*.cpp)

include_directories(${OPENNI_INCLUDE_DIR})
include_directories(${OPENNI_MODULES_DIR}/Common/)

add_library(nimCommon ${nimCommon_SOURCES})

if(nimCommon_COMPILE_FLAGS)
    set_property(TARGET nimCommon PROPERTY COMPILE_FLAGS ${nimCommon_COMPILE_FLAGS})
endif()

target_link_libraries(nimCommon OpenNI)

install(TARGETS nimCommon EXPORT openni-exports
	ARCHIVE DESTINATION lib
)

