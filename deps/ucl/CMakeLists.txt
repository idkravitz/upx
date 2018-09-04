cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

project(ucl)

# In order to compile on Windows under MSVC snprintf and vsnprintf defines should be removed
# Accomplish it by patching
if (MSVC)
	if (EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/ucl/acc/acc_auto.h)
		if (NOT EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/ucl/acc/acc_auto.h.orig)
			file(RENAME ${CMAKE_CURRENT_SOURCE_DIR}/ucl/acc/acc_auto.h ${CMAKE_CURRENT_SOURCE_DIR}/ucl/acc/acc_auto.h.orig)
			message(STATUS "Renaming")
			message(STATUS "    {CMAKE_CURRENT_SOURCE_DIR}/acc/acc_auto.h")
			message(STATUS "to acc_auto.h.orig because this file requires patching")
		endif()
	endif()
	if (EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/ucl/acc/acc_auto.h.orig)
		if (NOT EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/ucl/acc/acc_auto.h)
			file(READ ${CMAKE_CURRENT_SOURCE_DIR}/ucl/acc/acc_auto.h.orig _acc_auto_h_contents)
			string(REGEX REPLACE "# *define *v*snprintf  *_v*snprintf" "" _acc_auto_h_contents "${_acc_auto_h_contents}")
			file(WRITE "${CMAKE_CURRENT_SOURCE_DIR}/ucl/acc/acc_auto.h" "${_acc_auto_h_contents}")
			message(STATUS "Apply snprintf redefines removal patching to acc_auto.h")
		endif()
	endif()
	
	add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif()

aux_source_directory(${CMAKE_CURRENT_SOURCE_DIR}/ucl/src SRC_LIST)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/ucl)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/ucl/include)

add_library(uclstatic STATIC ${SRC_LIST})