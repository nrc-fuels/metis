cmake_minimum_required(VERSION 2.8)
project(METIS
  LANGUAGES C)

set(GKLIB_PATH "${CMAKE_CURRENT_LIST_DIR}/GKlib" CACHE PATH "path to GKlib")
set(BUILD_SHARED_LIBS FALSE CACHE BOOL "build a shared library")

if(MSVC)
  set(METIS_INSTALL FALSE)
else()
  set(METIS_INSTALL TRUE)
endif()

# Configure libmetis library.
if(BUILD_SHARED_LIBS)
  set(METIS_LIBRARY_TYPE SHARED)
else()
  set(METIS_LIBRARY_TYPE STATIC)
endif()

add_definitions(-DHAVE_RINT)
# include(CheckIncludeFile)
# CHECK_INCLUDE_FILE(math.h HAVE_MATH_H)
# if(${HAVE_MATH_H})
#   set(MATH_H_LOC ${HAVE_MATH_H})
# else()
#   set(MATH_H_LOC math.h)
# endif()
# include(CheckSymbolExists)
# check_symbol_exists(rint ${MATH_H_LOC} HAVE_RINT)
# message(STATUS "Have rint? ${HAVE_RINT}")
# if (${HAVE_RINT})
# 	add_definitions(-DHAVE_RINT)
# endif()

include(${GKLIB_PATH}/GKlibSystem.cmake)
# Add include directories.
include_directories(${GKLIB_PATH})
include_directories(include)
# Recursively look for CMakeLists.txt in subdirs.
add_subdirectory("include")
add_subdirectory("libmetis")
add_subdirectory("programs")
