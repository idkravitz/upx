cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

add_subdirectory(deps/zlib)
add_subdirectory(deps/ucl)
set(ZLIB_CONF_DIR ${CMAKE_CURRENT_BINARY_DIR}/deps/zlib)
add_subdirectory(src)