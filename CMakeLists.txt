cmake_minimum_required(VERSION 3.18)
project(visualization.projectm)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR})

find_package(Kodi REQUIRED)

include_directories(${includes}
                    ${KODI_INCLUDE_DIR}/..) # Hack way with "/..", need bigger Kodi cmake rework to match right include ways

if(NOT WIN32)
  find_package(projectM4 REQUIRED COMPONENTS Playlist)

  list(APPEND DEPLIBS libprojectM::playlist)

  set(PM_SOURCES src/Main.cpp)
  set(PM_HEADERS src/Main.h)
else()
  # WARNING: As Angle about to be used on Windows, we need to use OpenGL ES 3.0, but ProjectM 4.x.x
  # is only compatible with OpenGL ES 3.2, so we need to use the older version of ProjectM which
  # is compatible with OpenGL 3.0.
  find_package(Angle REQUIRED)
  find_package(glm REQUIRED)
  find_package(ProjectM REQUIRED)

  list(APPEND DEPLIBS ${OPENGLES_LIBRARIES}
                      ${PROJECTM_LIBRARIES})

  set(PM_SOURCES src/Main_WinV3.cpp)
  set(PM_HEADERS src/Main_WinV3.h)
endif()

include_directories(${includes}
                    ${PROJECTM_INCLUDE_DIRS}
                    ${KODI_INCLUDE_DIR}/..) # Hack way with "/..", need bigger Kodi cmake rework to match right include ways

build_addon(visualization.projectm PM DEPLIBS)

include(CPack)
