# This code is released under the
# Apache License Version 2.0 http://www.apache.org/licenses/.
#
# Copyright (c) 2012 Louis Dionne
#
cmake_minimum_required(VERSION 2.8.7)

project(LIBYOJIMBO CXX C)

include_directories(windows)
include_directories(.)

file(GLOB HPP_FILES yojimbo_*.h)
file(GLOB CPP_FILES yojimbo_*.cpp)
file(GLOB SODIUM_FILES windows/*.cpp)

set(YOJIMBO_SRC yojimbo.h
		yojimbo.cpp
		${HPP_FILES}
		${CPP_FILES}
		tlsf/tlsf.h
		tlsf/tlsf.c
)
# lib yojimbo
add_library(yojimbo STATIC ${YOJIMBO_SRC})
target_link_libraries(yojimbo sodium mbedtls mbedx509 mbedcrypto)

# info
add_executable(test_info tests/info.cpp)
target_link_libraries(test_info yojimbo)

# test
add_executable(test_yojimbo tests/test.cpp)
target_link_libraries(test_yojimbo yojimbo)

# client
add_executable(client tests/client.cpp
                      tests/shared.h
                      )
target_link_libraries(client yojimbo)
