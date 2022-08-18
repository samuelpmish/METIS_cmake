cmake_minimum_required(VERSION 3.8)
project(METIS_PROJ LANGUAGES C)

set(CMAKE_EXPORT_COMPILE_COMMANDS 1)

set(CMAKE_C_STANDARD 11)

#find_package(GKlib REQUIRED )
add_subdirectory(../GKlib_cmake ${CMAKE_CURRENT_BINARY_DIR}/GKlib)
#target_include_directories(mfem PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/TPL/mfem>)

option(USE_64_BIT_REAL    "Use 64-bit floating point numbers" ON)
option(USE_64_BIT_INTEGER "Use 64-bit signed integers" ON)

file(GLOB sources ${PROJECT_SOURCE_DIR}/src/*.c)
file(GLOB headers ${PROJECT_SOURCE_DIR}/include/*.h)

add_library(METIS ${sources})
target_include_directories(METIS PUBLIC ${PROJECT_SOURCE_DIR}/include)
target_link_libraries(METIS PUBLIC GKlib)

if (USE_64_BIT_REAL)
  target_compile_definitions(METIS PUBLIC USE_64_BIT_REAL)
endif()

if (USE_64_BIT_REAL)
  target_compile_definitions(METIS PUBLIC USE_64_BIT_INTEGER)
endif()

# mtest.c doesn't compile (?)
#
#if (ENABLE_TESTING)
#  enable_testing()
#  add_executable(mtest tests/mtest.c)
#  add_test(mtest ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/mtest)		
#  target_link_libraries(mtest PUBLIC METIS)
#endif()

if (ENABLE_PROGRAMS)
  add_subdirectory(programs)
endif()