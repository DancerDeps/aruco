# ----------------------------------------------------------------------------
#   Basic Configuration
# ----------------------------------------------------------------------------
cmake_minimum_required(VERSION 3.0)
project(aruco VERSION "3.1.2" LANGUAGES CXX)
set(PROJECT_SOVERSION "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}")
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_CXX_STANDARD 11) # C++11...
set(CMAKE_CXX_STANDARD_REQUIRED ON) #...is required...
set(CMAKE_CXX_EXTENSIONS ON) #...with compiler extensions like gnu++11

include(cmake/options.cmake)

include(cmake/findDependencies.cmake)
include(cmake/compilerOptions.cmake)

# catkin support
find_package(catkin REQUIRED)
catkin_package(
  INCLUDE_DIRS src
  LIBRARIES aruco
#  CATKIN_DEPENDS other_catkin_pkg
#  DEPENDS system_lib
)
include_directories(src)
OPTION(BUILD_UTILS  "Set to OFF to not compile utils " OFF)
OPTION(BUILD_TESTS  "Set to OFF to not compile tests " OFF)
OPTION(BUILD_GLSAMPLES  "Set to OFF to not compile GL samples " OFF)

add_subdirectory(src)

if(GL_FOUND AND BUILD_GLSAMPLES)
  add_subdirectory(utils_gl)
endif()
if(BUILD_TESTS)
  add_subdirectory(tests)
endif()


if(BUILD_UTILS)
  add_subdirectory(utils)
  add_subdirectory(utils_markermap)
  add_subdirectory(utils_calibration)
  IF(BUILD_SVM)
  ADD_SUBDIRECTORY(utils_svm)
  ENDIF()
  ADD_SUBDIRECTORY(utils_fractal)
endif()
include(cmake/printInfo.cmake)
include(cmake/installOptions.cmake)

# Package Generator  #######################################################
IF(BUILD_DEBPACKAGE)
include(cmake/cpack.cmake)
ENDIF()
