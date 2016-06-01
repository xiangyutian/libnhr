#
#   Copyright (c) 2016 Kulykov Oleh <info@resident.name>
#
#   Permission is hereby granted, free of charge, to any person obtaining a copy
#   of this software and associated documentation files (the "Software"), to deal
#   in the Software without restriction, including without limitation the rights
#   to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
#   copies of the Software, and to permit persons to whom the Software is
#   furnished to do so, subject to the following conditions:
#
#   The above copyright notice and this permission notice shall be included in
#   all copies or substantial portions of the Software.
#
#   THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
#   IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
#   FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
#   AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
#   LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
#   OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
#   THE SOFTWARE.
#

include(CheckLibraryExists)

include_directories(${PROJECT_BINARY_DIR})

link_directories(${PROJECT_BINARY_DIR})

remove_definitions(-DCMAKE_BUILD)

if (NHR_OPT_APPVEYOR_CI)
	add_definitions(-DNHR_APPVEYOR_CI)
endif()

add_executable(test_libnhr_creation test_libnhr_creation.c)
#set_property(TARGET test_libnhr_creation APPEND PROPERTY COMPILE_FLAGS -DLIBNHR_STATIC)
target_link_libraries(test_libnhr_creation nhr)
add_test(test_libnhr_creation test_libnhr_creation)



if(NHR_HAVE_PTHREAD_H)
	target_link_libraries(test_libnhr_creation pthread)
endif(NHR_HAVE_PTHREAD_H)

if(MINGW)
	target_link_libraries(test_libnhr_creation ws2_32)
endif(MINGW)


install(TARGETS test_libnhr_creation DESTINATION bin)
