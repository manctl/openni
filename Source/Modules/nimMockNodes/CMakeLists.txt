#project(nimMockNodes)

file(GLOB nimMockNodes_sources ./*.cpp)

include_directories(${OPENNI_INCLUDE_DIR})
include_directories(${OPENNI_MODULES_DIR}/Common/)

add_library(nimMockNodes SHARED ${nimMockNodes_sources})
target_link_libraries(nimMockNodes OpenNI)

install(TARGETS nimMockNodes EXPORT openni-exports
	LIBRARY DESTINATION lib
	RUNTIME DESTINATION bin
)
