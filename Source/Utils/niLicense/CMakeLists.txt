set(LICENSE_EXE niLicense)

file(GLOB ${LICENSE_EXE}_sources ./*.cpp)

include_directories(${OPENNI_INCLUDE_DIR})

add_executable(${LICENSE_EXE} ${${LICENSE_EXE}_sources})

target_link_libraries(${LICENSE_EXE} OpenNI)

add_dependencies(${LICENSE_EXE} OpenNI)

