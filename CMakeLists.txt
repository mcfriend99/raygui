cmake_minimum_required(VERSION 3.18)
project(raygui C)

set(CMAKE_C_STANDARD 99)

cmake_policy(SET CMP0026 OLD)

add_library(raygui SHARED raygui.c raygui.h)

add_subdirectory(raylib)
add_definitions(-DRAYGUI_IMPLEMENTATION)
add_definitions(-DBUILD_LIBTYPE_SHARED)
add_definitions(-DBUILD_SHARED_LIBS)

target_include_directories(raygui PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/raylib/src")
add_dependencies(raygui raylib)

if(${CMAKE_SYSTEM_NAME} STREQUAL "Linux")
  message(STATUS "Building with -fPIC support")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC -Wno-unused-result -Wno-stringop-overflow -Wno-alloc-size-larger-than")
#  target_compile_options(raygui PRIVATE -fPIC -Wno-unused-result -Wno-stringop-overflow -Wno-alloc-size-larger-than)
endif()

get_target_property(RAYLIB_STATIC_LIB raylib LOCATION)
target_link_libraries(raygui raylib)
