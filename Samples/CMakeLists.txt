include_directories(${OPENNI_INCLUDE_DIR})

find_library(GLUT_LIB glut)

if(DEFINED OPENNI_PLATFORM_APPLE)
    find_library(OPENGL_LIB OpenGL)
endif(DEFINED OPENNI_PLATFORM_APPLE)

#===============================================================================

file(GLOB NiAudioSample_SOURCES NiAudioSample/*.cpp)

add_executable(NiAudioSample ${NiAudioSample_SOURCES})

target_link_libraries(NiAudioSample OpenNI)

#-------------------------------------------------------------------------------

file(GLOB NiBackRecorder_SOURCES NiBackRecorder/*.cpp)

add_executable(NiBackRecorder ${NiBackRecorder_SOURCES})

target_link_libraries(NiBackRecorder OpenNI)

#-------------------------------------------------------------------------------

file(GLOB NiConvertXToONI_SOURCES NiConvertXToONI/*.cpp)

add_executable(NiConvertXToONI ${NiConvertXToONI_SOURCES})

target_link_libraries(NiConvertXToONI OpenNI)

#-------------------------------------------------------------------------------

# FIXME: NiCRead compiles and runs on Mac but gives error in linux.

if(FALSE)
    file(GLOB NiCRead_SOURCES NiCRead/*.c)
    
    add_executable(NiCRead ${NiCRead_SOURCES})
    
    target_link_libraries(NiCRead OpenNI)
endif()

#-------------------------------------------------------------------------------

file(GLOB NiRecordSynthetic_SOURCES NiRecordSynthetic/*.cpp)

add_executable(NiRecordSynthetic ${NiRecordSynthetic_SOURCES})

target_link_libraries(NiRecordSynthetic OpenNI ${GLUT_LIB})

#-------------------------------------------------------------------------------

file(GLOB NiSampleModule_SOURCES NiSampleModule/*.cpp)

add_library(NiSampleModule SHARED ${NiSampleModule_SOURCES})

target_link_libraries(NiSampleModule OpenNI)

#-------------------------------------------------------------------------------

file(GLOB NiSimpleCreate_SOURCES NiSimpleCreate/*.cpp)

add_executable(NiSimpleCreate ${NiSimpleCreate_SOURCES})

target_link_libraries(NiSimpleCreate OpenNI)

#-------------------------------------------------------------------------------

file(GLOB NiSimpleRead_SOURCES NiSimpleRead/*.cpp)

add_executable(NiSimpleRead ${NiSimpleRead_SOURCES})

target_link_libraries(NiSimpleRead OpenNI)

#-------------------------------------------------------------------------------

file(GLOB NiSimpleViewer_SOURCES NiSimpleViewer/*.cpp)

add_executable(NiSimpleViewer ${NiSimpleViewer_SOURCES})

target_link_libraries(NiSimpleViewer OpenNI ${GLUT_LIB} ${OPENGL_LIB})

#-------------------------------------------------------------------------------

# FIXME: NiUserTracker is disabled because of opengles.

if(FALSE)
    file(GLOB NiUserTracker_SOURCES ./*.cpp)
    
    add_executable(NiUserTracker ${NiUserTracker_SOURCES})
    
    target_link_libraries(NiUserTracker
        OpenNI
        ${GLUT_LIB}
        ${OPENGL_LIB}
    )
endif()

#-------------------------------------------------------------------------------

file(GLOB NiViewer_SOURCES NiViewer/*.cpp)

include_directories(NiViewer)

add_executable(NiViewer ${NiViewer_SOURCES})

target_link_libraries(NiViewer 
    OpenNI 
    ${GLUT_LIB} 
    ${OPENGL_LIB}
)

#===============================================================================

install(TARGETS
    NiAudioSample
    NiBackRecorder
    NiConvertXToONI
    NiRecordSynthetic
    NiSampleModule
    NiSimpleCreate
    NiSimpleRead
    NiSimpleViewer
    NiViewer

    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)
