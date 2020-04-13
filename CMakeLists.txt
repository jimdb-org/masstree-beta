cmake_minimum_required(VERSION 2.6)
project(masstree C CXX)

set(CONFIG_HDR ${PROJECT_SOURCE_DIR}/config.h)

if (NOT EXISTS ${CONFIG_HDR})
    add_custom_command(
            OUTPUT ${CONFIG_HDR}
            COMMAND ./bootstrap.sh
            COMMAND env CXXFLAGS=-std=c++11 ./configure --disable-superpage --disable-preconditions --enable-max-key-len=1048576
            DEPENDS ${PROJECT_SOURCE_DIR}/configure.ac
            WORKING_DIRECTORY ${PROJECT_SOURCE_DIR})
endif()

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -g -O2 -include ${CMAKE_CURRENT_SOURCE_DIR}/config.h")

set(kvtreeSRCS
    ${CONFIG_HDR}
    json.cc
    straccum.cc
    string.cc
    clp.c
    compiler.cc
    memdebug.cc
    kvthread.cc
)
add_library(mass-tree STATIC ${kvtreeSRCS})
