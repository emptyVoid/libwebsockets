project(lws-minimal-dbus-ws-proxy)
cmake_minimum_required(VERSION 2.8)
include(CheckCSourceCompiles)
include(CheckLibraryExists)
include(LwsCheckRequirements)

set(SAMP lws-minimal-dbus-ws-proxy)
set(SRCS main.c)

set(requirements 1)
require_lws_config(LWS_ROLE_DBUS 1 requirements)
require_lws_config(LWS_ROLE_WS 1 requirements)
require_lws_config(LWS_WITH_SERVER 1 requirements)
require_lws_config(LWS_WITH_CLIENT 1 requirements)

if (NOT MSVC AND NOT WIN32 AND requirements)
	add_executable(${SAMP} ${SRCS})

	if (NOT LWS_PLAT_FREERTOS)
		find_package(PkgConfig QUIET)
		pkg_check_modules(PC_DBUS1 dbus-1 QUIET)
		list(APPEND LWS_DBUS_INCLUDE1 ${PC_DBUS1_INCLUDE_DIRS})
		list(APPEND LWS_DBUS_LIB "${PC_DBUS1_LIBRARIES};dl")
	endif()

	include_directories("${LWS_DBUS_INCLUDE1}")

	if (websockets_shared)
		target_link_libraries(${SAMP} websockets_shared)
		add_dependencies(${SAMP} websockets_shared ${LWS_DBUS_LIB})
	else()
		target_link_libraries(${SAMP} websockets ${LWS_DBUS_LIB})
	endif()
endif()
