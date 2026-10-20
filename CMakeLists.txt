cmake_minimum_required(VERSION 3.20)
project(VJGenerative CXX)
set(CMAKE_CXX_STANDARD 17)
# Runtime estatico: o usuario NAO precisa instalar o Visual C++ Redistributable
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

include(FetchContent)
FetchContent_Declare(ffgl GIT_REPOSITORY https://github.com/resolume/ffgl.git GIT_TAG master)
FetchContent_GetProperties(ffgl)
if(NOT ffgl_POPULATED)
  FetchContent_Populate(ffgl)
endif()

file(GLOB FFGL_SRC
  ${ffgl_SOURCE_DIR}/source/lib/ffgl/*.cpp
  ${ffgl_SOURCE_DIR}/source/lib/ffglex/*.cpp)

add_library(VJGenerative SHARED VJGenerative.cpp ${FFGL_SRC})
target_include_directories(VJGenerative PRIVATE ${ffgl_SOURCE_DIR}/source/lib ${CMAKE_CURRENT_SOURCE_DIR})
target_compile_definitions(VJGenerative PRIVATE NOMINMAX _CRT_SECURE_NO_WARNINGS)
target_link_libraries(VJGenerative PRIVATE opengl32)
set_target_properties(VJGenerative PROPERTIES OUTPUT_NAME "VJGenerativeV1")
