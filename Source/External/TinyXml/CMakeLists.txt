if(UNIX AND CMAKE_SYSTEM_NAME MATCHES Linux)
    set(LINUX ON)
endif()
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(M64 ON)
else()
    set(M32 ON)
endif()

set(TinyXml_SOURCES
    tinyxml.cpp
    tinystr.cpp
    tinyxmlerror.cpp
    tinyxmlparser.cpp
)

if(LINUX AND M64)
    list(APPEND TinyXml_COMPILE_FLAGS "-fPIC")
endif()

add_library(TinyXml STATIC ${TinyXml_SOURCES})

if(TinyXml_COMPILE_FLAGS)
    set_property(TARGET TinyXml PROPERTY COMPILE_FLAGS ${TinyXml_COMPILE_FLAGS})
endif()

install(TARGETS TinyXml EXPORT openni-exports DESTINATION lib)
