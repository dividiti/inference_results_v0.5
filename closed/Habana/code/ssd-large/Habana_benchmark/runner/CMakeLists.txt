cmake_minimum_required(VERSION 3.5.1)
project(runner_lib)
if(${ENABLE_SANITIZER} STREQUAL "ON")
    add_compile_options("${SANITIZER_FLAGS}")
endif()
set(API_SOURCE_DIR "${$CMAKE_CURRENT_LIST_DIR}/source")
file(GLOB_RECURSE API_SOURCES *.cpp)
set(TARGET_PATH "${CMAKE_CURRENT_LIST_DIR}/../benchmark_app/lib")
add_library(runner_lib STATIC ${API_SOURCES})
target_include_directories(runner_lib PUBLIC "${CMAKE_CURRENT_LIST_DIR}/../global_headers" "${CMAKE_CURRENT_LIST_DIR}/headers/" "$ENV{MLPERF_LOADGEN_PATH}" "$ENV{MLPERF_LOADGEN_PATH}/bindings" "/usr/lib/habanalabs/include")
add_custom_command(TARGET runner_lib  POST_BUILD COMMAND "${CMAKE_COMMAND}" -E copy "$<TARGET_FILE:runner_lib>" "${TARGET_PATH}/${CMAKE_BUILD_TYPE}/$<TARGET_FILE_NAME:runner_lib>" COMMENT "Copying to output directory")