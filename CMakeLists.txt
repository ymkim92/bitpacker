cmake_minimum_required(VERSION 3.15)
project(bitpacker
    VERSION 0.1.0
    DESCRIPTION "Bit level serialization"
    LANGUAGES C CXX
)

# Add the library as header-only
add_library(bitpacker INTERFACE)
add_library(bitpacker::bitpacker ALIAS bitpacker)

option(BITPACKER_USE_CXX17 "Enable C++17 features for bitpacker" ON)
if(BITPACKER_USE_CXX17)
    message("Using C++17 bitpacker features...")
    target_compile_features(bitpacker INTERFACE cxx_std_17)
else()
    message("Using Bitpacker in C++14 mode...")
    target_compile_features(bitpacker INTERFACE cxx_std_14)
endif()

target_include_directories(bitpacker INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

# Install the library
install(
    TARGETS bitpacker
    EXPORT bitpackerTargets
    INCLUDES DESTINATION include
)

# Install the headers
install(DIRECTORY include/ DESTINATION include)

# Export the targets to a file
install(
    EXPORT bitpackerTargets
    NAMESPACE bitpacker::
    DESTINATION share/bitpacker
)

# Generate the config file for find_package
include(CMakePackageConfigHelpers)

configure_package_config_file(
    "${CMAKE_CURRENT_LIST_DIR}/cmake/bitpacker-config.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/bitpacker-config.cmake"
    INSTALL_DESTINATION share/bitpacker
)

write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/bitpacker-config-version.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

install(
    FILES
        "${CMAKE_CURRENT_BINARY_DIR}/bitpacker-config.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/bitpacker-config-version.cmake"
    DESTINATION share/bitpacker
)
