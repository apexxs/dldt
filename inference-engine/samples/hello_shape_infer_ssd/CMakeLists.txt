# Copyright (c) 2018 Intel Corporation

# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at

#      http://www.apache.org/licenses/LICENSE-2.0

# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
cmake_minimum_required(VERSION 2.8)

set(TARGET_NAME "hello_shape_infer_ssd")

file(GLOB SRC
        ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp
        )

file(GLOB HEADERS
        ${CMAKE_CURRENT_SOURCE_DIR}/*.hpp
        )

# Find OpenCV components if exist
find_package(OpenCV COMPONENTS imgcodecs QUIET)
if(NOT(OpenCV_FOUND))
    message(WARNING "OPENCV is disabled or not found, " ${TARGET_NAME} " skipped")
    return()
endif()

# Create named folders for the sources within the .vcproj
# Empty name lists them directly under the .vcproj
source_group("src" FILES ${SRC})
source_group("headers" FILES ${HEADERS})

link_directories(${LIB_FOLDER})

# Create library file from sources.
add_executable(${TARGET_NAME} ${SRC} ${HEADERS})

set_target_properties(${TARGET_NAME} PROPERTIES COMPILE_PDB_NAME ${TARGET_NAME})

target_link_libraries(${TARGET_NAME} ${InferenceEngine_LIBRARIES} IE::ie_cpu_extension ${OpenCV_LIBRARIES})

if (UNIX)
    target_link_libraries(${TARGET_NAME} ${LIB_DL})
endif ()
