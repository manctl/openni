add_subdirectory(Common)
add_subdirectory(nimCodecs)
add_subdirectory(nimMockNodes)
add_subdirectory(nimRecorder)

add_dependencies(nimCommon OpenNI)

add_dependencies(nimCodecs    nimCommon jpeg)
add_dependencies(nimMockNodes nimCommon)
add_dependencies(nimRecorder  nimCommon)
