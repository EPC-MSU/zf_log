cmake_minimum_required(VERSION 2.8)

macro(use_c99)
	if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu99")
    else ()
    	set(CMAKE_C_STANDARD 99)
  	endif ()
endmacro(use_c99)

use_c99()

# zf_log target (required)
set(HEADERS_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(HEADERS zf_log.h)
set(SOURCES zf_log.c)

if(MSVC)
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /W4 /WX")
else()
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra -Werror -pedantic-errors")
endif()

add_library(zf_log ${HEADERS} ${SOURCES})
target_include_directories(zf_log PUBLIC $<BUILD_INTERFACE:${HEADERS_DIR}>)

# install (optional)
if(ZF_LOG_CONFIGURE_INSTALL)
	if(NOT DEFINED INSTALL_INCLUDE_DIR)
		set(INSTALL_INCLUDE_DIR include)
	endif()
	if(NOT DEFINED INSTALL_LIB_DIR)
		set(INSTALL_LIB_DIR lib)
	endif()
	install(TARGETS zf_log EXPORT zf_log
		INCLUDES DESTINATION ${INSTALL_INCLUDE_DIR}
		ARCHIVE DESTINATION ${INSTALL_LIB_DIR})
	install(DIRECTORY ${HEADERS_DIR}/
		DESTINATION ${INSTALL_INCLUDE_DIR}
		FILES_MATCHING PATTERN "zf_*.h*")
endif()
