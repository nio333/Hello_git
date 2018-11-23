cmake_minimum_required(VERSION 3.9.0)
# set(CMAKE_BUILD_TYPE Release)
set(CMAKE_BUILD_TYPE Debug)
                                                        
#  _____                            _____     _           
# |     |___ _____ _____ ___ ___   |   __|___| |_ _ _ ___ 
# |   --| . |     |     | . |   |  |__   | -_|  _| | | . |
# |_____|___|_|_|_|_|_|_|___|_|_|  |_____|___|_| |___|  _|
#                                                    |_|  
# GIT versioning
execute_process(
  COMMAND git describe --long --dirty=§
  WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}
  OUTPUT_VARIABLE GIT_COMMIT_HASH
  OUTPUT_STRIP_TRAILING_WHITESPACE
)
execute_process(
  COMMAND git describe --abbrev=0 --tags
  WORKING_DIRECTORY ${CMAKE_HOME_DIRECTORY}
  OUTPUT_VARIABLE GIT_VERSION_TAG
  OUTPUT_STRIP_TRAILING_WHITESPACE
)

project(pub VERSION ${GIT_VERSION_TAG} LANGUAGES C)

# Common setup for all targets:
include_directories(/usr/local/include) #search for headers in that dir
set(SOURCE_DIR src)
file(GLOB LIB_SOURCES "${SOURCE_DIR}/*.c") #put into LIB_SOURCES the globbed list
file(GLOB LIB_HEADERS "${SOURCE_DIR}/*.h") # headers
if(UNIX AND NOT APPLE)
  set(LINUX_SOURCE_DIR src/linux)
  file(GLOB LINUX_LIB_SOURCES "${LINUX_SOURCE_DIR}/*.c")
  message(STATUS "Linux specific sources added to static lib: ${LINUX_LIB_SOURCES}")
endif()

# Header file containing the version
configure_file(
  ${CMAKE_HOME_DIRECTORY}/src/version.h.in
  ${CMAKE_HOME_DIRECTORY}/src/version.h
)