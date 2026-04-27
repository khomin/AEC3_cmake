cmake_minimum_required(VERSION 3.0)

set(PROJECT webrtc_aec3)

set(CMAKE_CXX_STANDARD 20)

project(${PROJECT} VERSION 0.1.0)

add_compile_options(-Wall -fPIC -Wno-deprecated -m64 -fexceptions)

if(APPLE)
    add_definitions(-DWEBRTC_MAC -DWEBRTC_POSIX)
elseif(UNIX AND NOT APPLE)
    add_definitions(-DWEBRTC_LINUX -DWEBRTC_POSIX)
elseif(WIN32)
    add_definitions(-DWEBRTC_WIN)
endif()

file(GLOB AECM_SRC
    ${CMAKE_CURRENT_SOURCE_DIR}/api/*.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/*.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/*.c
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/aec3/*.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/aec3/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/include/*.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/include/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/logging/*.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/logging/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/resampler/*.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/resampler/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/base/system_wrappers/source/*.c
    ${CMAKE_CURRENT_SOURCE_DIR}/base/system_wrappers/source/*.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/base/system_wrappers/include/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/base/rtc_base/memory/*.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/base/rtc_base/memory/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/base/rtc_base/*.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/base/rtc_base/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/base/rtc_base/strings/*.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/base/rtc_base/strings/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/base/rtc_base/system/*.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/base/rtc_base/system/*.h
    ${CMAKE_CURRENT_SOURCE_DIR}/base/jsoncpp/src/lib_json/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/base/jsoncpp/src/lib_json/*.h
)
list(FILTER AECM_SRC EXCLUDE REGEX ".*unittest.cc$")
set(SRC_FILES ${AECM_SRC}
    ${CMAKE_CURRENT_SOURCE_DIR}/base/rtc_base/checks.h
    ${CMAKE_CURRENT_SOURCE_DIR}/base/rtc_base/time_utils.h
    ${CMAKE_CURRENT_SOURCE_DIR}/base/rtc_base/sanitizer.h
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/utility/cascaded_biquad_filter.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/utility/ooura_fft.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/utility/ooura_fft_mips.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/utility/ooura_fft_neon.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/utility/ooura_fft_sse2.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/utility/cascaded_biquad_filter.h
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/utility/ooura_fft.h
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/utility/ooura_fft_tables_common.h
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/utility/ooura_fft_tables_neon_sse2.h
)

add_library(${PROJECT} STATIC
    ${SRC_FILES}
)

target_include_directories(${PROJECT} PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/
    ${CMAKE_CURRENT_SOURCE_DIR}/api
    ${CMAKE_CURRENT_SOURCE_DIR}/base/rtc_base
    ${CMAKE_CURRENT_SOURCE_DIR}/base
    ${CMAKE_CURRENT_SOURCE_DIR}/base/system_wrappers/include
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing
    ${CMAKE_CURRENT_SOURCE_DIR}/base/abseil
    ${CMAKE_CURRENT_SOURCE_DIR}/base/jsoncpp/include
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/aec3/
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/resampler
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/logging
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/include
    ${CMAKE_CURRENT_SOURCE_DIR}/audio_processing/utility
)

set_target_properties(${PROJECT} PROPERTIES LINKER_LANGUAGE CXX)

install(TARGETS ${PROJECT}
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
)