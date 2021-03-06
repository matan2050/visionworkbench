cmake_policy(SET CMP0025 NEW) # needed for conda build on OSX with conda

cmake_minimum_required (VERSION 3.2)

# Make it possible to append to these from the command line
set(CMAKE_CXX_FLAGS "" CACHE STRING "")
set(CMAKE_C_FLAGS "" CACHE STRING "")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -g -std=c++11")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3 -g")
set(CMAKE_EXE_LINKER_FLAGS "-L${BINARYBUILDER_INSTALL_DIR}/lib -lm -pthread -lpthread")

if(APPLE)
    # A workaround for the clang included with conda build
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -mlinker-version=305")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mlinker-version=305")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -mlinker-version=305")
else(APPLE)
    # On Linux need to link to rt
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -lrt")
endif(APPLE)

# Tell cmake to look in the /cmake folder.
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

# Make sure we do a release type
set(CMAKE_BUILD_TYPE "Release")
message("Setting CMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}")

project(visionworkbench)

add_subdirectory(src)

