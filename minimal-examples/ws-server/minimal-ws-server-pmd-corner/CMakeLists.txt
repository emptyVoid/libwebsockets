project(lws-minimal-ws-server-pmd-corner)
cmake_minimum_required(VERSION 2.8.9)
include(CheckCSourceCompiles)
include(LwsCheckRequirements)

set(SAMP lws-minimal-ws-server-pmd-corner)
set(SRCS minimal-ws-server-pmd-corner.c)

set(requirements 1)
require_lws_config(LWS_ROLE_WS 1 requirements)
require_lws_config(LWS_WITH_SERVER 1 requirements)
require_lws_config(LWS_WITHOUT_EXTENSIONS 0 requirements)

if (requirements)
	add_executable(${SAMP} ${SRCS})

	if (websockets_shared)
		target_link_libraries(${SAMP} websockets_shared)
		add_dependencies(${SAMP} websockets_shared)
	else()
		target_link_libraries(${SAMP} websockets)
	endif()
endif()
