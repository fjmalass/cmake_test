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

# specify the C++ standard
add_library(example_compiler_flags INTERFACE)
target_compile_features(example_compiler_flags INTERFACE cxx_std_23)

set(CMAKE_CXX_COMPILER clang++)

# use --log-level= (ERROR, WARNING, NOTICE, STATUS, VERBOSE, DEBUG, TRACE) or using 
# CMAKE_MESSAGE_LOG_LEVEL
message(
    CHECK_START
    "${CMAKE_CXX_COMPILER} Global Compile Options and Definitions"
)
set(gcc_like_cxx "$<COMPILE_LANG_AND_ID:CXX,ARMClang,AppleClang,Clang,GNU,LCC>")
set(msvc_cxx "$<COMPILE_LANG_AND_ID:CXX,MSVC>")
add_compile_options(
    "$<${gcc_like_cxx}:$<BUILD_INTERFACE:-Wall;-Wextra;-Wshadow;-Wformat=2;-Wunused>>"
    "$<${msvc_cxx}:$<BUILD_INTERFACE:-W3>>"
)
# Rem: Debug, RelWithDebInfo
message(VERBOSE "CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}")
if(CMAKE_BUILD_TYPE MATCHES "Deb")
    # target_compile_definitions(Example PRIVATE DEBUG)
    add_compile_definitions(DEBUG)
    message(VERBOSE "Add Definition: -DDEBUG")
endif()
# Rem: Debug only
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    add_compile_options(-Og)
    message(VERBOSE "Add Option: -Og")
endif()

# Rem: Release, RelWithDebugInfo 
if(CMAKE_BUILD_TYPE MATCHES "Rel")
    add_compile_options(-O3)
    message(VERBOSE "Add Option: -O3")
endif()
message(CHECK_PASS "Done")

#
# Set different compile options based on build type
# add_compile_options($<$<CONFIG:Debug>:-Og -g -DDEBUG> $<$<CONFIG:Release>:-O3>)

add_executable(Example example.cpp)

# Used the CMake Config VERSION etc to .h file
configure_file(ExampleConfig.h.in ExampleConfig.h)
target_include_directories(Example PUBLIC "${PROJECT_BINARY_DIR}")

# Add a DEBUG preprocessor definition for the Debug configuration (CMAKE_BUILD_TYPE)
# Needed as makefiles only support single configuration

# add new library  WIP
# add_subdirectory(MathFunctions) # expects another CMakeLists.txt in subdiretory
# list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
# target_link_libraries(Example PUBLIC MathFunctions)

target_include_directories(
    Example
    PUBLIC
        "${PROJECT_BINARY_DIR}" # "${EXTRA_INCLUDES}"
)
# installation in bin
install(TARGETS Example DESTINATION bin)
install(FILES ${PROJECT_BINARY_DIR}/ExampleConfig.h DESTINATION include)
install(FILES ${PROJECT_SOURCE_DIR}/README.md DESTINATION docs)

# Should include tests 
