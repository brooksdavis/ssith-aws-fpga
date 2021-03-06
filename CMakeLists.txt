cmake_minimum_required(VERSION 3.10)
project(ssith_aws_fpga)

if(FPGA)
message(NOTICE " Building for FPGA")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DDEFAULT_NOPROGRAM=1 -DBOARD_awsf1 -g")
set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -DDEFAULT_NOPROGRAM=1 -DBOARD_awsf1 -g")
else()
message(NOTICE " Building for simulation")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DSIMULATION -DBOARD_verilator -g")
set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -DSIMULATION -DBOARD_verilator -g")
endif()

add_subdirectory(src/tinyemu)
add_subdirectory(src/connectal)
add_subdirectory(src/generated)

set(CMAKE_CXX_STANDARD 14)

include_directories(src)
include_directories(src/tinyemu)
include_directories(src/connectal)
include_directories(src/generated)

add_executable(ssith_aws_fpga
  src/fpga.h
  src/fpga.cpp
  src/loadelf.cpp
  src/loadelf.h
  src/util.cpp
  src/util.h
  src/virtiodevices.cpp
  src/virtiodevices.h
  src/main.cpp)
target_link_libraries(ssith_aws_fpga tinyemu connectal generated pthread elf)

add_executable(ssith_virtio_test
  src/fpga.h
  src/fpga.cpp
  src/loadelf.cpp
  src/loadelf.h
  src/util.cpp
  src/util.h
  src/virtiodevices.cpp
  src/virtiodevices.h
  src/test/virtiotest.cpp
  )
target_link_libraries(ssith_virtio_test tinyemu connectal generated pthread elf)

