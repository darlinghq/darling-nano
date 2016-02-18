project(nano)

cmake_minimum_required(VERSION 2.4.0)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
	cmake_policy(SET CMP0005 NEW)
endif(COMMAND cmake_policy)

include(darling_exe)
add_definitions(-DTARGET_OS_MAC=1)
add_definitions(-D__APPLE__ -D__DYNAMIC__)
add_definitions(-DHAVE_CONFIG_H)
add_definitions(-DSYSCONFDIR="${CMAKE_INSTALL_PREFIX}/share/darling")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -U_POSIX_C_SOURCE -D__DARWIN_UNIX03 -nostdinc -fblocks -fPIC -ggdb -O0")
set(CMAKE_EXE_LINKER_FLAGS "-nodefaultlibs -nostdlib -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")
#set(CMAKE_SHARED_LINKER_FLAGS "-nodefaultlibs -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")

include_directories(${DARLING_TOP_DIRECTORY}/src/libc/include/FreeBSD)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/include/NetBSD)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/gen)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/darwin)
include_directories(${DARLING_TOP_DIRECTORY}/src/libc/stdtime/FreeBSD)
include_directories(${DARLING_TOP_DIRECTORY}/src/launchd/liblaunch)
include_directories(${DARLING_TOP_DIRECTORY}/src/external/libdispatch)
include_directories(${DARLING_TOP_DIRECTORY}/src/external/zlib)
include_directories(${DARLING_TOP_DIRECTORY}/src/external/bzip2)
include_directories(${DARLING_TOP_DIRECTORY}/src/ncurses/include)
include_directories(BEFORE ${CMAKE_CURRENT_SOURCE_DIR})

include(InstallSymlink)
set(BINARY_PACKAGING_MODE ON)

set(nano_sources
	src/browser.c
	src/chars.c
	src/color.c
	src/cut.c
	src/files.c
	src/global.c
	src/help.c
	src/move.c
	src/nano.c
	src/prompt.c
	src/rcfile.c
	src/search.c
	src/text.c
	src/utils.c
	src/winio.c
)

add_darling_executable(nano ${nano_sources})

target_link_libraries(nano ncurses)

install(TARGETS nano
	DESTINATION libexec/darling/usr/bin)
install(FILES nanorc
	DESTINATION share/darling)

