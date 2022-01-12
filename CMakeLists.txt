cmake_minimum_required(VERSION 3.13)

project(bitwise)

set(CMAKE_EXPORT_COMPILE_COMMANDS on)

file(GLOB_RECURSE SRC
 ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c)
file(GLOB CHIPMUNK_SRC
 ${CMAKE_CURRENT_SOURCE_DIR}/lib/Chipmunk2D/src/*.c)

add_executable(bitwise
 ${SRC}
 ${CHIPMUNK_SRC})

if(MINGW)
 target_compile_options(bitwise PRIVATE
  -Dmain=SDL_main)
endif()

target_compile_options(bitwise PRIVATE
 -fdiagnostics-color=always)

if (CMAKE_C_COMPILER_ID STREQUAL "Clang")
 target_compile_options(bitwise PRIVATE
  -Weverything
  -Wpedantic)
elseif (CMAKE_C_COMPILER_ID STREQUAL "GNU")
 target_compile_options(bitwise PRIVATE
  -Wall
  -Wextra
  -Wshadow
  -Wpedantic
  -Wduplicated-cond
  -Wduplicated-branches
  -Wlogical-op)
elseif (CMAKE_C_COMPILER_ID STREQUAL "MSVC")
 target_compile_options(bitwise PRIVATE
  /W4)
endif()

target_link_directories(bitwise PRIVATE
build)

if(MINGW)
 target_link_libraries(bitwise PRIVATE
  mingw32
  SDL2main)
elseif(UNIX)
 target_link_libraries(bitwise PRIVATE
  pthread)
endif()

target_link_libraries(bitwise PRIVATE
 m
 SDL2
 SDL2_image 
 SDL2_mixer
 SDL2_net
 SDL2_ttf)

target_include_directories(bitwise PRIVATE
 SYSTEM ${CMAKE_CURRENT_SOURCE_DIR}/lib/SDL2_image-2.0.5/x86_64-w64-mingw32/include/SDL2
 SYSTEM ${CMAKE_CURRENT_SOURCE_DIR}/lib/SDL2_mixer-2.0.4/x86_64-w64-mingw32/include/SDL2
 SYSTEM ${CMAKE_CURRENT_SOURCE_DIR}/lib/SDL2_net-2.0.1/x86_64-w64-mingw32/include/SDL2
 SYSTEM ${CMAKE_CURRENT_SOURCE_DIR}/lib/SDL2_ttf-2.0.15/x86_64-w64-mingw32/include/SDL2
 SYSTEM ${CMAKE_CURRENT_SOURCE_DIR}/lib/SDL2-2.0.18/x86_64-w64-mingw32/include/SDL2
 SYSTEM ${CMAKE_CURRENT_SOURCE_DIR}/lib/Chipmunk2D/include
 SYSTEM ${CMAKE_CURRENT_SOURCE_DIR}/lib/minicoro)
