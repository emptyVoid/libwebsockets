project(lws-minimal-http-server-cgi)
cmake_minimum_required(VERSION 2.8)
include(CheckCSourceCompiles)
include(LwsCheckRequirements)

set(SAMP lws-minimal-http-server-cgi)
set(SRCS minimal-http-server.c)

set(requirements 1)
require_lws_config(LWS_ROLE_H1 1 requirements)
require_lws_config(LWS_WITH_CGI 1 requirements)
require_lws_config(LWS_WITHOUT_SERVER 0 requirements)

if (requirements)
	add_executable(${SAMP} ${SRCS})

	if (websockets_shared)
		target_link_libraries(${SAMP} websockets_shared)
		add_dependencies(${SAMP} websockets_shared)
	else()
		target_link_libraries(${SAMP} websockets)
	endif()
endif()
