cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

project(upx)

# add_subdirectory(deps/zlib)

aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR} SRC_LIST)
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(../deps/ucl/ucl/include)
include_directories(lzma-sdk)
include_directories(../deps/zlib)
include_directories(${ZLIB_CONF_DIR})
set(EXECUTABLE upx)

file(GLOB_RECURSE HEADERS "*.h")

add_definitions(-DUCL_NO_ASM -D_CRT_SECURE_NO_WARNINGS)
set(CMAKE_CXX_FLAGS "-MT -EHsc -J -O2 -W4 -WX")

add_executable(${EXECUTABLE} ${SRC_LIST} ${HEADERS})
target_link_libraries(${EXECUTABLE} zlibstatic uclstatic)
