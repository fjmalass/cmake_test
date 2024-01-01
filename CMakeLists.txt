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
# add compiler warning flags just when building this project via
# the BUILD_INTERFACE genex

set(gcc_like_cxx "$<COMPILE_LANG_AND_ID:CXX,ARMClang,AppleClang,Clang,GNU,LCC>")
set(msvc_cxx "$<COMPILE_LANG_AND_ID:CXX,MSVC>")
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    message("GCC: ${gcc_like_cxx}")
    message("MSVC: ${msvc_cxx}")
endif()
add_compile_options(
    "$<${gcc_like_cxx}:$<BUILD_INTERFACE:-Wall;-Wextra;-Wshadow;-Wformat=2;-Wunused>>"
    "$<${msvc_cxx}:$<BUILD_INTERFACE:-W3>>"
)

# Add a DEBUG preprocessor definition for the Debug configuration (--config Debug)
add_compile_definitions("$<$<CONFIG:Debug>:DEBUG>")
#
# Set different compile options based on build type
# add_compile_options($<$<CONFIG:Debug>:-Og -g -DDEBUG> $<$<CONFIG:Release>:-O3>)

add_executable(Example example.cpp)

# Used the CMake Config VERSION etc to .h file
configure_file(ExampleConfig.h.in ExampleConfig.h)
target_include_directories(Example PUBLIC "${PROJECT_BINARY_DIR}")

# add new library  WIP
# add_subdirectory(MathFunctions)
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
