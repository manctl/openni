include_directories(${TINYXML_INCLUDE_DIR})
include_directories(${OPENNI_INCLUDE_DIR})
include_directories("./FilesOnly")

file(GLOB OpenNI_sources ./*.cpp ./*.h ../../Include/*.h)

if(OPENNI_PLATFORM_LINUX)
    file(GLOB OpenNI_platform_sources ./Linux/*.cpp)
elseif(OPENNI_PLATFORM_APPLE)
    file(GLOB OpenNI_platform_sources ./Linux/*.cpp)
    set(OPENNI_PLATFORM_LIBS)
elseif(OPENNI_PLATFORM_WIN32)
    file(GLOB OpenNI_platform_sources ./WIN32/*.cpp)
    set(OPENNI_PLATFORM_LIBS Ws2_32.lib Setupapi.lib)
    include_directories(${OpenNI_SOURCE_DIR}/Platform/Win32/Driver/Build/sys)
    add_definitions(-DXN_EXPORTS)
endif()

add_library(OpenNI SHARED 
    ${OpenNI_sources}
    ${OpenNI_platform_sources}
)

set_property(TARGET OpenNI PROPERTY LINK_INTERFACE_LIBRARIES "")

if(UNIX)
    find_library(LIBDL dl)
endif()

target_link_libraries(OpenNI
    usb
    ${LIBDL}
    TinyXml
    ${OPENNI_PLATFORM_LIBS}
)

if(WIN32 AND M64)
    set_property(TARGET OpenNI PROPERTY OUTPUT_NAME OpenNI64)
endif()

install(TARGETS OpenNI EXPORT openni-exports
    RUNTIME DESTINATION bin
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)
