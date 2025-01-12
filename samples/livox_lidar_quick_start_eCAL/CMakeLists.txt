cmake_minimum_required(VERSION 3.0)
set(CMAKE_FIND_PACKAGE_PREFER_CONFIG ON) # needed for eCAL


if (MSVC)
  set(CMAKE_CXX_STANDARD 11)             # needed for eCAL
  set(CMAKE_CXX_STANDARD_REQUIRED ON)    # needed for eCAL
endif()


find_package(eCAL REQUIRED)
find_package(Protobuf REQUIRED)

set(source_files
    main.cpp
    pointcloud/pointcloudhandler.cpp
)

include_directories(.
    pointcloud
)

set(protobuf_files
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/pcl.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/pcl_header.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/pcl_pfield.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/types.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/lane.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/foxglove/LinePrimitive.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/foxglove/Color.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/foxglove/Pose.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/foxglove/Point3.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/foxglove/Quaternion.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/foxglove/Vector3.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/foxglove/PointCloud.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/proto/foxglove/PackedElementField.proto
)

set(DEMO_NAME livox_lidar_quick_start_eCAL)

#target_include_directories(.)

# Option A)
#project(livox_lidar_quick_start_eCAL)
#PROTOBUF_TARGET_CPP(${PROJECT_NAME} ${CMAKE_CURRENT_SOURCE_DIR}/proto/ ${protobuf_files})
#set(_protobuf_include_path ${CMAKE_CURRENT_SOURCE_DIR}/proto)
#EXEC_PROGRAM(${PROTOBUF_PROTOC_EXECUTABLE} ARGS --cpp_out ${CMAKE_CURRENT_BINARY_DIR} ${_protobuf_include_path} ${protobuf_files})
#add_executable(${DEMO_NAME} main.cpp)
add_executable(${DEMO_NAME} ${source_files})
# the below doesn't create the c++ files from protos atm. believe this is due to sub-project
#    however it creates the folder structures in the VC++ Studio solution
#    proto files are created manually atm by executing the compile_proto.bat, followed by copy of the .proto files into build folder
PROTOBUF_TARGET_CPP(${DEMO_NAME} ${CMAKE_CURRENT_SOURCE_DIR}/proto/ ${protobuf_files})

# Option B)
#set(source_files main.cpp)
#include_directories(${CMAKE_CURRENT_BINARY_DIR})
#protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS ${protobuf_files})
#add_executable(${DEMO_NAME} ${source_files} ${PROTO_SRCS} ${PROTO_HDRS})


target_link_libraries(${DEMO_NAME}
    PUBLIC
    livox_lidar_sdk_static
    eCAL::core
    protobuf::libprotobuf
    )
