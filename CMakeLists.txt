#
# MIT License
#
# Copyright (c) 2021 Olivier Le Doeuff
#
# Permission is hereby granted, free of charge, to any person obtaining a copy of this software and
# associated documentation files (the "Software"), to deal in the Software without restriction,
# including without limitation the rights to use, copy, modify, merge, publish, distribute,
# sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all copies or
# substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT
# NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
# NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM,
# DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT
# OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
#

cmake_minimum_required(VERSION 3.14.0...3.24.0)

project(
  asio.cmake
  VERSION 1.3.1
  LANGUAGES CXX
)

# Define PROJECT_IS_TOP_LEVEL if not available
if(CMAKE_VERSION VERSION_LESS "3.21")
  string(COMPARE EQUAL "${CMAKE_CURRENT_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}" PROJECT_IS_TOP_LEVEL)
endif()

#
# Options
#
include(CMakeDependentOption)
set(ASIO_REPOSITORY
    "https://github.com/chriskohlhoff/asio"
    CACHE STRING "asio git repository url"
)
set(ASIO_TAG
    "asio-1-34-2"
    CACHE STRING "asio git tag"
)
option(ASIO_USE_CPM "Download Asio with CPM instead of FetchContent" ON)
option(
  ASIO_CPM_FETCHCONTENT_COMPAT
  "Should asio be declared with FetchContent functions to be compatible. This doesn't not allow CPM cache to work."
  OFF
)
option(ASIO_NO_DEPRECATED "Disables Asio deprecated interfaces and functionality" ON)
option(ASIO_SEPARATE_COMPILATION "Uses separately compiled source code for Asio's implementation"
  ON
)
if(PROJECT_IS_TOP_LEVEL AND BUILD_SHARED_LIBS)
  set(ASIO_DYN_LINK_DEFAULT_VALUE ON)
else()
  set(ASIO_DYN_LINK_DEFAULT_VALUE OFF)
endif()
cmake_dependent_option(
  ASIO_DYN_LINK
  "Uses separately compiled source code for Asio's implementation, with symbols exported for inclusion as part of a shared library"
  ${ASIO_DYN_LINK_DEFAULT_VALUE}
  "ASIO_SEPARATE_COMPILATION"
  OFF
)
option(ASIO_ENABLE_EXAMPLES "add one example to test binding/compilation" ${PROJECT_IS_TOP_LEVEL})
option(ASIO_ENABLE_INSTALL "Install the asio standalone library" ${PROJECT_IS_TOP_LEVEL})

#
# Library type
#
if(NOT ASIO_SEPARATE_COMPILATION)
  set(ASIO_LIBRARY_TYPE INTERFACE)
elseif(NOT ASIO_DYN_LINK)
  set(ASIO_LIBRARY_TYPE STATIC)
else()
  set(ASIO_LIBRARY_TYPE SHARED)
endif()

# Declare ##

if(ASIO_CPM_FETCHCONTENT_COMPAT OR NOT ASIO_USE_CPM)
  include(FetchContent)

  FetchContent_Declare(
    asio
    GIT_REPOSITORY ${ASIO_REPOSITORY}
    GIT_TAG ${ASIO_TAG}
  )

  FetchContent_GetProperties(asio)
endif()

if(ASIO_ENABLE_INSTALL)
  include(GNUInstallDirs) # for CMAKE_INSTALL_INCLUDEDIR
endif()

if(NOT asio_POPULATED AND NOT TARGET asio)

  # Download ##

  if(ASIO_USE_CPM)
    # Include CPM only if not already initialized
    get_property(
      CPM_INITIALIZED GLOBAL ""
      PROPERTY CPM_INITIALIZED
      SET
    )
    if(NOT CPM_INITIALIZED)
      include(cmake/get_cpm.cmake)
    endif()
    CPMAddPackage(
      NAME asio
      GIT_REPOSITORY ${ASIO_REPOSITORY}
      GIT_TAG ${ASIO_TAG}
      DOWNLOAD_ONLY TRUE
    )
  else()
    message(STATUS "Download asio from ${ASIO_REPOSITORY}:${ASIO_TAG}")
    FetchContent_Populate(asio)
  endif()

  # Find Version ##
  file(STRINGS "${asio_SOURCE_DIR}/asio/include/asio/version.hpp" ASIO_VERSION
       REGEX "^#define ASIO_VERSION [0-9]+"
  )
  string(REGEX REPLACE "^#define ASIO_VERSION [0-9]+ // ([0-9.]+)$" "\\1" ASIO_VERSION
                       "${ASIO_VERSION}"
  )

  set(ASIO_VERSION
      ${ASIO_VERSION}
      CACHE STRING "asio library version" FORCE
  )

  # Library scopes
  if(ASIO_LIBRARY_TYPE STREQUAL INTERFACE)
    set(ASIO_TARGET_INCLUDE_DIRECTORIES_SCOPE INTERFACE)
    set(ASIO_TARGET_LINK_LIBRARIES_SCOPE INTERFACE)
    set(ASIO_TARGET_COMPILE_FEATURES_SCOPE INTERFACE)
    set(ASIO_TARGET_COMPILE_DEFINITIONS_SCOPE INTERFACE)
  else()
    set(ASIO_TARGET_INCLUDE_DIRECTORIES_SCOPE PUBLIC)
    set(ASIO_TARGET_LINK_LIBRARIES_SCOPE PRIVATE)
    set(ASIO_TARGET_COMPILE_FEATURES_SCOPE PUBLIC)
    set(ASIO_TARGET_COMPILE_DEFINITIONS_SCOPE PUBLIC)
  endif()

  # Configure ##

  add_library(
    asio ${ASIO_LIBRARY_TYPE} $<$<NOT:$<STREQUAL:${ASIO_LIBRARY_TYPE},INTERFACE>>:src/asio.cpp>
  )
  add_library(asio::asio ALIAS asio)
  target_include_directories(
    asio SYSTEM ${ASIO_TARGET_INCLUDE_DIRECTORIES_SCOPE}
    $<BUILD_INTERFACE:${asio_SOURCE_DIR}/asio/include>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
  )

  find_package(Threads)
  target_link_libraries(asio ${ASIO_TARGET_LINK_LIBRARIES_SCOPE} Threads::Threads)

  target_compile_features(asio ${ASIO_TARGET_COMPILE_FEATURES_SCOPE} cxx_std_11)
  target_compile_definitions(
    asio
    ${ASIO_TARGET_COMPILE_DEFINITIONS_SCOPE}
    -DASIO_STANDALONE
    $<$<NOT:$<STREQUAL:${ASIO_LIBRARY_TYPE},INTERFACE>>:ASIO_SEPARATE_COMPILATION>
    $<$<STREQUAL:${ASIO_LIBRARY_TYPE},SHARED>:ASIO_DYN_LINK>
    $<$<BOOL:${ASIO_NO_DEPRECATED}>:ASIO_NO_DEPRECATED>
  )

  # Fix warning : "Please define _WIN32_WINNT or _WIN32_WINDOWS appropriately."
  # https://stackoverflow.com/questions/9742003/platform-detection-in-cmake
  if(WIN32 AND CMAKE_SYSTEM_VERSION)
    set(ver ${CMAKE_SYSTEM_VERSION})
    string(REPLACE "." "" ver ${ver})
    string(REGEX REPLACE "([0-9])" "0\\1" ver ${ver})
    set(version "0x${ver}")
    target_compile_definitions(
      asio ${ASIO_TARGET_COMPILE_DEFINITIONS_SCOPE} -D_WIN32_WINNT=${version}
    )
  endif()

endif()

#
# Examples
#
if(ASIO_ENABLE_EXAMPLES)
  enable_testing()
  add_executable(
    asio_example_cpp11_timers_time_t_timer
    ${asio_SOURCE_DIR}/asio/src/examples/cpp11/timers/time_t_timer.cpp
  )
  target_link_libraries(asio_example_cpp11_timers_time_t_timer PRIVATE asio::asio)
  add_custom_target(asio_examples DEPENDS asio_example_cpp11_timers_time_t_timer)
  add_test(NAME asio_example COMMAND asio_example_cpp11_timers_time_t_timer)
endif()

#
# Install
#
if(ASIO_ENABLE_INSTALL)
  if(NOT CPM_INITIALIZED)
    include(cmake/get_cpm.cmake)
  endif()
  CPMAddPackage("gh:TheLartians/PackageProject.cmake@1.12.0")
  packageProject(
    NAME asio
    VERSION ${ASIO_VERSION}
    NAMESPACE asio
    BINARY_DIR ${PROJECT_BINARY_DIR}
    INCLUDE_DIR ${asio_SOURCE_DIR}/asio/include
    INCLUDE_DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    DISABLE_VERSION_SUFFIX YES
    COMPATIBILITY SameMajorVersion
    DEPENDENCIES "Threads"
  )
endif()
