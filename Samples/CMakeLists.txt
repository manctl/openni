include_directories(${OPENNI_INCLUDE_DIR})

find_package(OpenGL REQUIRED)
list(APPEND EXTRA_LIBS ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY})

if(WIN32)
    # FIXME: Add glut as a proper dependency.
    if (CMAKE_CL_64)
        set(GLUT_DLL ${OpenNI_SOURCE_DIR}/Platform/Win32/Bin64/Release/glut64.dll)
        set(GLUT_LIB ${CMAKE_CURRENT_SOURCE_DIR}/NiViewer/Libs/glut64.lib)
    else()
        set(GLUT_DLL ${OpenNI_SOURCE_DIR}/Platform/Win32/Bin/Release/glut32.dll)
        set(GLUT_LIB ${CMAKE_CURRENT_SOURCE_DIR}/NiViewer/Libs/glut32.lib)
    endif()
    list(APPEND EXTRA_LIBS ${GLUT_LIB} winmm.lib)
else()
    find_package(GLUT REQUIRED)
    list(APPEND EXTRA_LIBS ${GLUT_glut_LIBRARY})
endif()

#===============================================================================

file(GLOB NiAudioSample_SOURCES NiAudioSample/*.cpp)

add_executable(NiAudioSample ${NiAudioSample_SOURCES})

target_link_libraries(NiAudioSample OpenNI ${EXTRA_LIBS})

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
    
    target_link_libraries(NiCRead OpenNI ${EXTRA_LIBS})
endif()

#-------------------------------------------------------------------------------

file(GLOB NiRecordSynthetic_SOURCES NiRecordSynthetic/*.cpp)

add_executable(NiRecordSynthetic ${NiRecordSynthetic_SOURCES})

target_link_libraries(NiRecordSynthetic OpenNI ${EXTRA_LIBS})

#-------------------------------------------------------------------------------

file(GLOB NiSampleModule_SOURCES NiSampleModule/*.cpp)

add_library(NiSampleModule SHARED ${NiSampleModule_SOURCES})

target_link_libraries(NiSampleModule OpenNI ${EXTRA_LIBS})

#-------------------------------------------------------------------------------

file(GLOB NiSimpleCreate_SOURCES NiSimpleCreate/*.cpp)

add_executable(NiSimpleCreate ${NiSimpleCreate_SOURCES})

target_link_libraries(NiSimpleCreate OpenNI ${EXTRA_LIBS})

#-------------------------------------------------------------------------------

file(GLOB NiSimpleRead_SOURCES NiSimpleRead/*.cpp)

add_executable(NiSimpleRead ${NiSimpleRead_SOURCES})

target_link_libraries(NiSimpleRead OpenNI ${EXTRA_LIBS})

#-------------------------------------------------------------------------------

file(GLOB NiSimpleViewer_SOURCES NiSimpleViewer/*.cpp)

add_executable(NiSimpleViewer ${NiSimpleViewer_SOURCES})

target_link_libraries(NiSimpleViewer OpenNI ${EXTRA_LIBS})

#-------------------------------------------------------------------------------

# FIXME: NiUserTracker is disabled because of opengles.

if(FALSE)
    file(GLOB NiUserTracker_SOURCES ./*.cpp)
    
    add_executable(NiUserTracker ${NiUserTracker_SOURCES})
    
    target_link_libraries(NiUserTracker OpenNI ${EXTRA_LIBS})
endif()

#-------------------------------------------------------------------------------

file(GLOB NiViewer_SOURCES NiViewer/*.cpp)

include_directories(NiViewer)

add_executable(NiViewer ${NiViewer_SOURCES})

target_link_libraries(NiViewer OpenNI ${EXTRA_LIBS})

#-------------------------------------------------------------------------------

add_executable(kinect-motors Kinect/kinect-motors.cpp)
target_link_libraries(kinect-motors OpenNI)

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
    kinect-motors

    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

if(GLUT_DLL)
    install(FILES ${GLUT_DLL} DESTINATION bin)
endif()
