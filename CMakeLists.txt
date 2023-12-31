# cmake version number
cmake_minimum_required(VERSION 3.28)

project(
    Example
    VERSION 0.2.0.0
    DESCRIPTION "Example CMake"
    HOMEPAGE_URL https://rhypt.com
)
# will generate CMAKE_PROJECT_VERSION
# PROJECT_VERSION
# PROJECT_VERSION_MAJOR
# <PROJECT_NAME>_VERSION
# <PROJECT_NAME>_VERSION_MAJOR
# PROJECT_DESCRIPTION

# Compiler configuration
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_CXX_COMPILER /usr/bin/clang++)

add_executable(Example example.cpp)

# Used the CMake Config VERSION etc to .h file
configure_file(ExampleConfig.h.in ExampleConfig.h)

target_include_directories(Example PUBLIC "${PROJECT_BINARY_DIR}")
