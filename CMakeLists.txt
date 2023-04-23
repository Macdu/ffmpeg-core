if (NOT DEFINED FFMPEG_CORE_NAME)
	set(FFMPEG_CORE_NAME ffmpeg)
endif()

add_library(${FFMPEG_CORE_NAME} INTERFACE)

target_include_directories(${FFMPEG_CORE_NAME} INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}/include")
if (WIN32 AND NOT MINGW)
	target_link_libraries(${FFMPEG_CORE_NAME} INTERFACE
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/windows/x86_64/avformat.lib"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/windows/x86_64/avcodec.lib"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/windows/x86_64/swscale.lib"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/windows/x86_64/avutil.lib"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/windows/x86_64/avfilter.lib"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/windows/x86_64/swresample.lib"
		"psapi;strmiids;uuid;oleaut32;shlwapi;ws2_32;ole32;user32;bcrypt")
elseif (APPLE)
	target_link_libraries(${FFMPEG_CORE_NAME} INTERFACE
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/macos/${ARCHITECTURE}/libavformat.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/macos/${ARCHITECTURE}/libavcodec.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/macos/${ARCHITECTURE}/libswscale.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/macos/${ARCHITECTURE}/libavutil.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/macos/${ARCHITECTURE}/libavfilter.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/macos/${ARCHITECTURE}/libswresample.a"
		"-framework CoreServices" "-framework CoreFoundation" "-framework AudioUnit"
		"-framework AudioToolbox" "-framework CoreAudio" "-framework CoreMedia"
		"-framework VideoToolbox" "-framework CoreVideo" "-framework Security")
elseif (ANDROID AND NOT USE_STATIC_ANDROID_FFMPEG)
	target_link_libraries(${FFMPEG_CORE_NAME} INTERFACE
		"${CMAKE_CURRENT_SOURCE_DIR}/shared/android/${ANDROID_ABI}/libavformat.so"
		"${CMAKE_CURRENT_SOURCE_DIR}/shared/android/${ANDROID_ABI}/libavcodec.so"
		"${CMAKE_CURRENT_SOURCE_DIR}/shared/android/${ANDROID_ABI}/libavdevice.so"
		"${CMAKE_CURRENT_SOURCE_DIR}/shared/android/${ANDROID_ABI}/libswscale.so"
		"${CMAKE_CURRENT_SOURCE_DIR}/shared/android/${ANDROID_ABI}/libavutil.so"
		"${CMAKE_CURRENT_SOURCE_DIR}/shared/android/${ANDROID_ABI}/libavfilter.so"
		"${CMAKE_CURRENT_SOURCE_DIR}/shared/android/${ANDROID_ABI}/libswresample.so")
elseif (ANDROID AND USE_STATIC_ANDROID_FFMPEG)
	target_link_libraries(${FFMPEG_CORE_NAME} INTERFACE
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/android/${ANDROID_ABI}/libavformat.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/android/${ANDROID_ABI}/libavcodec.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/android/${ANDROID_ABI}/libavdevice.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/android/${ANDROID_ABI}/libswscale.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/android/${ANDROID_ABI}/libavutil.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/android/${ANDROID_ABI}/libavfilter.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/android/${ANDROID_ABI}/libswresample.a")
elseif (UNIX)
	target_link_libraries(${FFMPEG_CORE_NAME} INTERFACE
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/linux/x86_64/libavformat.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/linux/x86_64/libavcodec.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/linux/x86_64/libswscale.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/linux/x86_64/libavutil.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/linux/x86_64/libavfilter.a"
		"${CMAKE_CURRENT_SOURCE_DIR}/lib/linux/x86_64/libswresample.a")
else ()
	message(FATAL_ERROR "No prebuilt was found for ffmpeg")
endif ()
