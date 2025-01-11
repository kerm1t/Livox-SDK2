cmake_minimum_required(VERSION 3.0)
set(CMAKE_FIND_PACKAGE_PREFER_CONFIG ON) # needed for eCAL


if (MSVC)
  set(CMAKE_CXX_STANDARD 14)             # needed for eCAL
  set(CMAKE_CXX_STANDARD_REQUIRED ON)    # needed for eCAL
endif()


find_package(eCAL REQUIRED)
find_package(Protobuf REQUIRED)



set(DEMO_NAME livox_lidar_quick_start_eCAL)

add_executable(${DEMO_NAME} main.cpp)

target_link_libraries(${DEMO_NAME}
        PUBLIC
        livox_lidar_sdk_static
	eCAL::core
        protobuf::libprotobuf
	)
