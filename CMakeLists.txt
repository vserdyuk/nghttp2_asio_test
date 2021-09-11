cmake_minimum_required(VERSION 3.15)

set(CMAKE_TOOLCHAIN_FILE ${CMAKE_CURRENT_SOURCE_DIR}/vcpkg/scripts/buildsystems/vcpkg.cmake
	CACHE STRING "Vcpkg toolchain file")

project(nghttp2_asio_test)

set(TARGET_NAME nghttp2_asio_test)

add_executable(${TARGET_NAME} main.cpp)

target_compile_features(${TARGET_NAME} PRIVATE cxx_std_17)

if(WIN32)
	# https://github.com/nghttp2/nghttp2/issues/616
	# With real project it is better to add such dirty fix on a single file rather than the whole target
	# very bad:
	#target_compile_definitions(${TARGET_NAME} PRIVATE ssize_t=SSIZE_T)
	# just bad:
	set_property(SOURCE main.cpp APPEND PROPERTY COMPILE_DEFINITIONS ssize_t=SSIZE_T)
endif()

# When building for Linux, the order is important: first libnghttp2_asio, then nghttp2.
# TODO create FindNGHTTP2.cmake to manage order via CMake imported targets.

# nghttp2_asio
find_path(NGHTTP2_ASIO_INCLUDE_DIR nghttp2/asio_http2_client.h)
target_include_directories(${TARGET_NAME} PUBLIC ${NGHTTP2_ASIO_INCLUDE_DIR})
find_library(NGHTTP2_ASIO_LIBRARY nghttp2_asio NAMES libnghttp2_asio.a REQUIRED)
target_link_libraries(${TARGET_NAME} PUBLIC ${NGHTTP2_ASIO_LIBRARY})

#nghttp2
find_path(NGHTTP2_INCLUDE_DIR nghttp2/nghttp2.h)
target_include_directories(${TARGET_NAME} PUBLIC ${NGHTTP2_INCLUDE_DIR})
find_library(NGHTTP2_LIBRARY nghttp2 NAMES libnghttp2.a REQUIRED)
target_link_libraries(${TARGET_NAME} PUBLIC ${NGHTTP2_LIBRARY})

find_package(OpenSSL REQUIRED)
target_link_libraries(${TARGET_NAME} PUBLIC OpenSSL::SSL OpenSSL::Crypto)

