cmake_minimum_required(VERSION 3.8)

project(soloud)

#add_definitions(-DWITH_WINMM)
add_definitions(-DWITH_WASAPI)			# low latency
#add_definitions(-DWITH_XAUDIO2)

add_definitions(-DDISABLE_MP3_WAV_FLAC)	# custom

include_directories(
	include
)

set(SOLOUD_SRCS)
file(GLOB SOLOUD_SRCS
	src/core/*.c*
	src/filter/*.c*
	src/audiosource/wav/*.c*
	#src/backend/winmm/*.c*
	src/backend/wasapi/*.c*
	#src/backend/xaudio2/*.c*
)

add_library(soloud STATIC
    ${SOLOUD_SRCS}
)
