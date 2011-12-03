if(UNIX AND CMAKE_SYSTEM_NAME MATCHES Linux)
    set(LINUX ON)
endif()
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(M64 ON)
else()
    set(M32 ON)
endif()

set(${TINYXML_LIB_NAME}_sources tinyxml.cpp tinystr.cpp tinyxmlerror.cpp tinyxmlparser.cpp)

if(LINUX AND M64)
        list(APPEND ${TINYXML_LIB_NAME}_COMPILE_FLAGS "-fPIC")
endif()

add_library(${TINYXML_LIB_NAME} STATIC ${${TINYXML_LIB_NAME}_sources})

if(${TINYXML_LIB_NAME}_COMPILE_FLAGS)
    set_property(TARGET ${TINYXML_LIB_NAME} PROPERTY COMPILE_FLAGS ${${TINYXML_LIB_NAME}_COMPILE_FLAGS})
endif()

install(TARGETS ${TINYXML_LIB_NAME} EXPORT openni-exports DESTINATION lib)
