project(lws-minimal-raw-netcat)
cmake_minimum_required(VERSION 2.8)
include(CheckCSourceCompiles)
include(LwsCheckRequirements)

set(SAMP lws-minimal-raw-netcat)
set(SRCS minimal-raw-netcat.c)

set(requirements 1)
require_lws_config(LWS_WITH_SERVER 1 requirements)

if (requirements)
	add_executable(${SAMP} ${SRCS})

	if (websockets_shared)
		target_link_libraries(${SAMP} websockets_shared)
		add_dependencies(${SAMP} websockets_shared)
	else()
		target_link_libraries(${SAMP} websockets)
	endif()
endif()
