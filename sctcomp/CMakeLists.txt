# Copyright 2015 The Etc2Comp Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

project(sctcomp)

include_directories(../etclib/Etc)
include_directories(../etclib/EtcCodec)
include_directories(../third_party/lodepng)
include_directories(../../Repos/ssr-projects/client/yuna)
include_directories(../../Repos/ssr-projects/client/yuna/external)
include_directories(../../Repos/ssr-projects/client/yuna/external/win32-specific/zlib/include)
include_directories(../../Repos/ssr-projects/client/yuna/external/xxhash)

link_directories(../../Repos/ssr-projects/client/yuna/external/win32-specific/zlib/prebuilt)

file(GLOB SOURCES
	${PROJECT_SOURCE_DIR}/*.h
	${PROJECT_SOURCE_DIR}/*.cpp
	../third_party/lodepng/*.h
	../third_party/lodepng/*.cpp
	../../Repos/ssr-projects/client/yuna/yuna/archive.h
	../../Repos/ssr-projects/client/yuna/yuna/archive.cpp
	../../Repos/ssr-projects/client/yuna/external/xxhash/*.h
	../../Repos/ssr-projects/client/yuna/external/xxhash/*.c
	../../Repos/ssr-projects/client/yuna/external/lz4/*.h
	../../Repos/ssr-projects/client/yuna/external/lz4/*.c)


add_executable(sctcomp ${SOURCES})

target_link_libraries (sctcomp etclib)
target_link_libraries (sctcomp libzlib)

