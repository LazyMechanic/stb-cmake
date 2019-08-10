cmake_minimum_required(VERSION 3.13)

set(_PROJECT_MAJOR_VERSION 0)
set(_PROJECT_MINOR_VERSION 1)
set(_PROJECT_PATCH_VERSION 0)
set(_PROJECT_VERSION
  ${_PROJECT_MAJOR_VERSION}.${_PROJECT_MINOR_VERSION}.${_PROJECT_PATCH_VERSION})

project(stb VERSION "${_PROJECT_VERSION}")

# ############################################################### #
# stb options ################################################### #
# ############################################################### #

option(stb_INSTALL "Generate stb installation target" ON)
option(stb_EXPORT "Export stb source tree as package" ON)
set(
    stb_INSTALL_CMAKE_PREFIX 
    "lib/cmake/stb"
    CACHE PATH "Installation directory for CMake files")
set(
    stb_INSTALL_INCLUDE_PREFIX 
    "include/stb" 
    CACHE PATH "Installation directory for header files")

# ############################################################### #
# Create interface target ####################################### #
# ############################################################### #
add_library(
    stb
    INTERFACE)

add_library(
    stb::stb
    ALIAS stb)

target_include_directories(
    stb
    INTERFACE $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
              $<INSTALL_INTERFACE:include>)

# ############################################################### #
# Create vorbis header ########################################## #
# ############################################################### #

# Create vorbis header
file(READ ${CMAKE_CURRENT_SOURCE_DIR}/stb/stb_vorbis.c STB_VORBIS_CONTENT)
configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/stb_vorbis.h.in" 
    "${CMAKE_CURRENT_SOURCE_DIR}/stb/stb_vorbis.h")

# ############################################################### #
# Installing #################################################### #
# ############################################################### #

if(stb_INSTALL)
    file(
        GLOB stb_HEADERS
        "${CMAKE_CURRENT_SOURCE_DIR}/stb/*.h")

    # Install headers
    install(
        FILES       ${stb_HEADERS}
        DESTINATION ${stb_INSTALL_INCLUDE_PREFIX})

    # Create export targets
    install(
        TARGETS stb
        EXPORT  stb-targets)

    set(stb-targets_FILE "stb-targets.cmake")

    # Create config-targets cmake file
    install(
        EXPORT      stb-targets
        FILE        ${stb-targets_FILE}
        NAMESPACE   stb::
        DESTINATION ${stb_INSTALL_CMAKE_PREFIX})

    # Create config files
    include(CMakePackageConfigHelpers)
    write_basic_package_version_file(
        "${PROJECT_BINARY_DIR}/stb-config-version.cmake"
        VERSION ${PROJECT_VERSION}
        COMPATIBILITY AnyNewerVersion)

    configure_package_config_file(
        "${CMAKE_CURRENT_SOURCE_DIR}/cmake/stb-config.cmake.in"
        "${PROJECT_BINARY_DIR}/stb-config.cmake"
        INSTALL_DESTINATION ${stb_INSTALL_CMAKE_PREFIX})

    # Install config files
    install(
        FILES
            "${PROJECT_BINARY_DIR}/stb-config.cmake"
            "${PROJECT_BINARY_DIR}/stb-config-version.cmake"
        DESTINATION ${stb_INSTALL_CMAKE_PREFIX})
endif()

if (stb_EXPORT)
	export(PACKAGE stb)
endif()