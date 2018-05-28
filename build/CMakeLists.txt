#-------------------------------------------------------
# moFileReader Main Build Script
#
# Defined Variables:
#   - COMPILE_DLL 
#     - ON  : Compiles the code as a shared Library
#     - OFF : Compiles the code as a static Library
#   - COMPILE_APP
#     - ON  : Compiles the application.
#     - OFF : Skip the application.
#   - BUILD_DEBUG
#     - ON  : Compiles Debug-Information into the output
#     - OFF : Optimizes the compilation with O2.
#
# Run cmake with -DVARNAME=ON/OFF to benefit from those
# possible settings.
#-------------------------------------------------------
cmake_minimum_required(VERSION 2.6)
project(moFileReader)

option(COMPILE_APP "Set this to ON if you want to compile the app (default)." ON)

# executable build directory
if( COMPILE_APP )
    add_subdirectory(bin)
endif()

# Let the user choose between static lib and dll
# To use it, call cmake -DCOMPILE_DLL=ON
option(COMPILE_DLL "Set this to ON if you want to compile the library as an DLL. When this is OFF, a static library is created (default)." OFF)

if ( NOT COMPILE_DLL )

    # Static build
    add_library(moFileReader STATIC
      ../src/moFileReader/moFileReader.cpp)

else ( COMPILE_DLL )

    # DLL
    add_definitions(-D_USRDLL -DMOFILE_EXPORTS)
    add_library(moFileReader SHARED
      ../src/moFileReader/moFileReader.cpp)

endif ()

# The main include directory
target_include_directories(
  moFileReader
  PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/../include>
  $<INSTALL_INTERFACE:include>
  )


install(
  TARGETS moFileReader
  DESTINATION lib
  EXPORT moFileReader
  )

install(
  DIRECTORY ../include/
  DESTINATION include
  )

install(
  EXPORT moFileReader
  DESTINATION lib/cmake/moFileReader/
  FILE moFileReader-config.cmake
  )
