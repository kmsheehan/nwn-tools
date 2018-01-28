cmake_minimum_required(VERSION 2.8)
project(nwn-tools)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
if(NOT CMAKE_CXX_STANDARD OR CMAKE_CXX_STANDARD LESS 11)
    set(CMAKE_CXX_STANDARD 11)
endif()

if (APPLE)
    set(CMAKE_MACOSX_RPATH 1)
endif()

find_package(BISON)
if (NOT BISON_FOUND)
	message(FATAL_ERROR "Cannot find bison, need bison installed to generate Parser/Lexer")
endif (NOT BISON_FOUND)

if (UNIX OR APPLE)
	#set(CMAKE_CXX_FLAGS "-Wall -Wno-unused -Wno-write-strings -Wno-deprecated -std=c++11")
	set(CMAKE_CXX_FLAGS " -w -O2 -m32 -std=c++11")
	#set(CMAKE_CXX_FLAGS "-g -w -m32 -std=c++11")
	#set(CMAKE_CXX_FLAGS "-w /EHsc")
endif()

if (MINGW OR CYGWIN)
    #set(CMAKE_CXX_FLAGS "-Wall -Wno-unused -Wno-write-strings -Wno-deprecated -std=c++11 -static")
    set(CMAKE_CXX_FLAGS " -w -O2 -m32 -std=c++11 -static")
    #set(CMAKE_CXX_FLAGS "-g -w -m32 -std=c++11 -static")
    #set(CMAKE_CXX_FLAGS "-w /EHsc")
endif()


if (BISON_VERSION VERSION_GREATER "3.0.0")
   add_definitions(-DNWN_BISON_3=1)
endif (BISON_VERSION VERSION_GREATER "3.0.0")

add_subdirectory(_docopt)
add_subdirectory(_MathLib)
add_subdirectory(_NmcLib)
add_subdirectory(_NwnLib)
add_subdirectory(_NscLib)
add_subdirectory(nwnnss)
add_subdirectory(nwnmdl)
