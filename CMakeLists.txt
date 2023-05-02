project(nano)

cmake_minimum_required(VERSION 3.10)

if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
	cmake_policy(SET CMP0005 NEW)
endif(COMMAND cmake_policy)

include(darling_exe)
add_definitions(-DHAVE_CONFIG_H)
add_definitions(-DSYSCONFDIR="/etc")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -U_POSIX_C_SOURCE -D__DARWIN_UNIX03 -nostdinc -fblocks -fPIC -ggdb -O0")

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

install(FILES nanorc DESTINATION libexec/darling/private/etc)

install(FILES doc/nano.1 doc/rnano.1 DESTINATION libexec/darling/usr/share/man/man1)
install(FILES doc/nanorc.5 DESTINATION libexec/darling/usr/share/man/man5)
