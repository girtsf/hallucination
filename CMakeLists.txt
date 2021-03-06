CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
PROJECT(Hallucination)

# disallow in-source build
# include(MacroEnsureOutOfSourceBuild)
# macro_ensure_out_of_source_build("${PROJECT_NAME} requires an out of source build. Please create a separate build directory and run 'cmake /path/to/${PROJECT_NAME} [options]' there.")

SET(EXECUTABLE_NAME hallucination)

IF(APPLE)
   FIND_LIBRARY(COCOA_FRAMEWORK Cocoa)
   FIND_LIBRARY(OPENGL_FRAMEWORK OpenGL )
   FIND_LIBRARY(IOKIT_FRAMEWORK IOKit )
   FIND_LIBRARY(COREVIDEO_FRAMEWORK CoreVideo )
   MARK_AS_ADVANCED (COCOA_FRAMEWORK
                     OPENGL_FRAMEWORK
                     IOKIT_FRAMEWORK
                     COREVIDEO_FRAMEWORK
                     APP_SERVICES_LIBRARY)
   SET(EXTRA_LIBS ${COCOA_FRAMEWORK} ${OPENGL_FRAMEWORK} ${IOKIT_FRAMEWORK} ${COREVIDEO_FRAMEWORK})
ELSE(APPLE)
   SET(EXTRA_LIBS GL GLU X11 pthread Xrandr Xi Xxf86vm)
ENDIF (APPLE)

SET(PROJECT_SRCS main.cc audio.cc controller.cc hair.cc hallucination.cc obj_reader.cc)

FIND_PATH(GLM_INCLUDE_DIR glm/glm.hpp PATHS third_party)

FIND_LIBRARY(GLFW_LIBRARY glfw3)
FIND_LIBRARY(PORTAUDIO_LIBRARY portaudio)
FIND_LIBRARY(AUBIO_LIBRARY aubio)
SET(THIRD_PARTY_LIBS ${GLFW_LIBRARY} ${PORTAUDIO_LIBRARY} ${AUBIO_LIBRARY})

ADD_EXECUTABLE( ${EXECUTABLE_NAME} ${PROJECT_SRCS} )
TARGET_LINK_LIBRARIES( ${EXECUTABLE_NAME} ${THIRD_PARTY_LIBS} ${EXTRA_LIBS} )
TARGET_INCLUDE_DIRECTORIES(${EXECUTABLE_NAME} PUBLIC ${GLM_INCLUDE_DIR})
