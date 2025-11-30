cmake_minimum_required(VERSION 3.24)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD 20)

# Set the project name and language
project(
    NutritionTracker
    VERSION 0.0.1
    LANGUAGES CXX C
)

include(FetchContent)
if(CMAKE_VERSION VERSION_GREATER_EQUAL "3.24.0")
    cmake_policy(SET CMP0135 NEW)
endif()

# Add project_options from https://github.com/aminya/project_options
# Change the version in the following URL to update the package (watch the releases of the repository for future updates)
set(PROJECT_OPTIONS_VERSION "v0.29.0")
FetchContent_Declare(
    _project_options
    URL https://github.com/aminya/project_options/archive/refs/tags/${PROJECT_OPTIONS_VERSION}.zip
)
FetchContent_MakeAvailable(_project_options)
include(${_project_options_SOURCE_DIR}/Index.cmake)

# Initialize project_options variable related to this project
# This overwrites `project_options` and sets `project_warnings`
# uncomment to enable the options. Some of them accept one or more inputs:
project_options(
    # ENABLE_CACHE
    # ENABLE_CLANG_TIDY
    # ENABLE_VS_ANALYSIS
    ENABLE_INTERPROCEDURAL_OPTIMIZATION
    ENABLE_NATIVE_OPTIMIZATION
    # ENABLE_DOXYGEN
    # ENABLE_COVERAGE
    # ENABLE_SANITIZER_ADDRESS
    # ENABLE_SANITIZER_UNDEFINED_BEHAVIOR
    # ENABLE_SANITIZER_THREAD
    # ENABLE_SANITIZER_MEMORY
    # ENABLE_CONTROL_FLOW_PROTECTION
    # ENABLE_STACK_PROTECTION
    # ENABLE_OVERFLOW_PROTECTION
    # ENABLE_ELF_PROTECTION
    # ENABLE_RUNTIME_SYMBOLS_RESOLUTION
    ENABLE_COMPILE_COMMANDS_SYMLINK
    # ENABLE_PCH
    # PCH_HEADERS
    # WARNINGS_AS_ERRORS
    # ENABLE_INCLUDE_WHAT_YOU_USE
    # ENABLE_GCC_ANALYZER
    # ENABLE_BUILD_WITH_TIME_TRACE
)

# Force colored build output for gcc/clang and clang-tidy
if (CMAKE_CXX_CLANG_TIDY)
    set(CMAKE_CXX_CLANG_TIDY "${CMAKE_CXX_CLANG_TIDY};--use-color")
endif()

# TODO: Install clang 16 to be able to build this and with libc++, then test if lldb pretty
# printing is any better than with stdlibc++
# if(${CMAKE_CXX_COMPILER_ID} STREQUAL "Clang")
#     target_compile_options(project_options INTERFACE -stdlib=libc++)
#     target_link_libraries(project_options INTERFACE c++ c++abi)
# endif()

add_library(imgui STATIC
    vendor/imgui/imgui.cpp
    vendor/imgui/imgui_demo.cpp
    vendor/imgui/imgui_draw.cpp
    vendor/imgui/imgui_tables.cpp
    vendor/imgui/imgui_widgets.cpp
    vendor/imgui/backends/imgui_impl_sdl2.cpp
    vendor/imgui/backends/imgui_impl_opengl3.cpp
)

target_include_directories(imgui PUBLIC vendor/imgui vendor/imgui/backends)
target_link_libraries(imgui PRIVATE SDL2::SDL2)

add_subdirectory(vendor/SDL)
add_subdirectory(vendor/fmt)
add_subdirectory(vendor/range-v3)
add_subdirectory(vendor/nlohmann-json)

add_subdirectory(src)
