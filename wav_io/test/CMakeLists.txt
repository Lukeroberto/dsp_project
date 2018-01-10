cmake_minimum_required(VERSION 3.5)

project(wav_io)

# Thread Support
find_package(Threads REQUIRED)

# Enable the ExternalProject Module
include(ExternalProject)

# Download 
ExternalProject_Add(
    gtest
    URL https://github.com/google/googletest/archive/master.zip
    PREFIX ${CMAKE_CURRENT_BINARY_DIR}/gtest

    # Disable Install Step
    INSTALL_COMMAND ""
)

# Get GTest source and binary directories from CMake project
ExternalProject_Get_Property(gtest source_dir binary_dir)

# Create a libgtest target to be used as a dependency by test programs
add_library(libgtest IMPORTED STATIC GLOBAL)
add_dependencies(libgtest gtest)


# Set libgtest properties
set_target_properties(libgtest PROPERTIES
    "IMPORTED_LOCATION" "${binary_dir}/googlemock/gtest/libgtest.a"
    "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}"
)
include_directories("${source_dir}/googletest/include")

# Add test files
file(GLOB SRCS *.cpp)

# Print Test Files
message(${SRCS})

add_executable(tests ${SRCS})
target_link_libraries(tests libgtest)
install(TARGETS tests DESTINATION bin)
