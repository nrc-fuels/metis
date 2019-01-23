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

include(CheckIncludeFile)
CHECK_INCLUDE_FILE(math.h HAVE_MATH_H)
if(HAVE_MATH_H)
  set(INCLUDE_LINE "#include <math.h>")
endif()
include(CheckCSourceCompiles)
check_c_source_compiles(
  "
${INCLUDE_LINE}
int main(void){int a; a = rint(11.5); return 0;};
"
  RINT_COMPILES)
# rint() is never actually used. Introspection fails on Linux but hard
# override this for now.
if(RINT_COMPILES OR (${CMAKE_HOST_SYSTEM_NAME} MATCHES Linux))
  add_definitions(-DHAVE_RINT)
endif()

include(${GKLIB_PATH}/GKlibSystem.cmake)
# Add include directories.
include_directories(${GKLIB_PATH})
include_directories(include)
# Recursively look for CMakeLists.txt in subdirs.
add_subdirectory("include")
add_subdirectory("libmetis")
add_subdirectory("programs")
