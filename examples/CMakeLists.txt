
include_directories(example ${CMAKE_CURRENT_BINARY_DIR})

set(example_SRCS
    example.cpp
    example_pb2.py
)

protobuf_generate_cpp(example_PB_SRCS example_PB_HDRS "example.proto")

add_executable(example ${example_SRCS} ${example_PB_SRCS})
target_link_libraries(example Arcus)
set_target_properties(example PROPERTIES COMPILE_FLAGS "-std=c++11")

add_custom_command(
    OUTPUT example_pb2.py
    COMMAND  ${PROTOBUF_PROTOC_EXECUTABLE}
    ARGS --python_out ${CMAKE_CURRENT_BINARY_DIR} --proto_path=${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/example.proto
    COMMENT "Running Python protocol buffer compiler on example.proto"
    VERBATIM )

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/example_py.sh ${CMAKE_CURRENT_BINARY_DIR}/example_py.sh)
