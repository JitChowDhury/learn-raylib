# Minimum version required
cmake_minimum_required(VERSION 3.28)

# Project name and language
project(raylibTut LANGUAGES CXX)

# Where to put the final program
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Get raylib from GitHub
include(FetchContent)
FetchContent_Declare(raylib
    GIT_REPOSITORY https://github.com/raysan5/raylib.git
    GIT_TAG 5.5
    GIT_SHALLOW ON
    EXCLUDE_FROM_ALL
    SYSTEM
)
FetchContent_MakeAvailable(raylib)

# Collect sources
file(GLOB SRC_FILES src/*.cpp)
file(GLOB HEADER_FILES include/*.h)

# Define the executable
add_executable(raylibTut ${SRC_FILES} ${HEADER_FILES})

# Include and C++ standard
target_include_directories(raylibTut PRIVATE include)
target_compile_features(raylibTut PRIVATE cxx_std_17)

# Link raylib
target_link_libraries(raylibTut PRIVATE raylib)

# Copy raylib DLLs after build (Windows only)
if(WIN32)
    add_custom_command(TARGET raylibTut POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        $<TARGET_FILE_DIR:raylib> $<TARGET_FILE_DIR:raylibTut>
    )

    # set(APP_ICON_RESOURCE "${CMAKE_CURRENT_SOURCE_DIR}/resources.rc")
    # target_sources(raylibTut PRIVATE ${APP_ICON_RESOURCE})
endif()

# Compiler warnings
if(MSVC)
    target_compile_options(raylibTut PRIVATE /W4)
else()
    target_compile_options(raylibTut PRIVATE -Wall -Wextra -Wpedantic)
endif()
