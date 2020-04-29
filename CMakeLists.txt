cmake_minimum_required(VERSION 3.02)

include(cmake/Hunter/init.cmake)

project(jsonrpc-lean)

option(BUILD_EXAMPLES "Build example" OFF)

hunter_add_package(RapidJSON)
find_package(RapidJSON CONFIG REQUIRED)
find_path(RapidJSON_INCLUDE_DIR rapidjson)

include(cmake/install.cmake)

include_directories(SYSTEM ${PROJECT_SOURCE_DIR}/include)

include_directories(include)
include_directories(include/jsonrpc-lean)

include_directories(${RapidJSON_INCLUDE_DIR})

add_library(jsonrpc-lean INTERFACE)
jsonrpc_install(jsonrpc-lean)

if(BUILD_EXAMPLES)
  add_executable(testserver examples/testserver.cpp)
  add_executable(testclient examples/testclient.cpp)
  add_executable(base64 examples/base64.cpp)
else()
  # dummy target needs to avoiding error at install by hunter packages manager
  add_custom_target(dummy)
endif()
