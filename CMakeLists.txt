cmake_minimum_required(VERSION 3.24)

project(soloud)

include_directories(
		include
)

set(SOLOUD_SRCS)

if(WIN32)
	add_definitions(-DWITH_WINMM)
	#add_definitions(-DWITH_WASAPI)			# low latency but issue( un/replugin sound device will mute )
	#add_definitions(-DWITH_XAUDIO2)
	file(GLOB SOLOUD_SRCS
			src/core/*.c*
			src/filter/*.c*
			src/audiosource/wav/*.c*
			src/backend/winmm/*.c*
			#src/backend/wasapi/*.c*
			#src/backend/xaudio2/*.c*
	)
endif()

if(APPLE)
	add_definitions(-DWITH_COREAUDIO)
	file(GLOB SOLOUD_SRCS
			src/core/*.c*
			src/filter/*.c*
			src/audiosource/wav/*.c*
			src/backend/coreaudio/soloud_coreaudio.cpp
	)
endif()

add_library(soloud STATIC
		${SOLOUD_SRCS}
)

if(MSVC)
	add_compile_options($<$<PLATFORM_ID:Windows>:/MP${Biogears_BUILD_PARALLEL_LEVEL}>)
	target_compile_options(soloud PRIVATE
			$<$<PLATFORM_ID:Windows>:/MP${Biogears_BUILD_PARALLEL_LEVEL}>
	)
endif()