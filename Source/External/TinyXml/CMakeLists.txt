set(${TINYXML_LIB_NAME}_sources tinyxml.cpp tinystr.cpp tinyxmlerror.cpp tinyxmlparser.cpp)

add_library(${TINYXML_LIB_NAME} STATIC ${${TINYXML_LIB_NAME}_sources})

install(TARGETS ${TINYXML_LIB_NAME} EXPORT openni-exports DESTINATION lib)
