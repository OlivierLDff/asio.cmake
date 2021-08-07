#
# MIT License
#
# Copyright (c) 2021 Olivier Le Doeuff
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.
#

cmake_minimum_required(VERSION 3.11.0)

project(asio.cmake VERSION 1.0.0)

set(ASIO_REPOSITORY "https://github.com/chriskohlhoff/asio" CACHE STRING "asio git repository url")
set(ASIO_TAG "master" CACHE STRING "asio git tag")

option(ASIO_USE_CPM "Download Asio with CPM instead of FetchContent" OFF)

include(FetchContent)

## Declare ##

FetchContent_Declare(
  asio
  GIT_REPOSITORY ${ASIO_REPOSITORY}
  GIT_TAG        ${ASIO_TAG}
)

FetchContent_GetProperties(asio)

if(NOT asio_POPULATED AND NOT TARGET asio)

  ## Download ##

  if(ASIO_USE_CPM)
    include(cmake/CPM.cmake)
    CPMAddPackage(
      NAME asio
      GIT_REPOSITORY ${ASIO_REPOSITORY}
      GIT_TAG ${ASIO_TAG}
      DOWNLOAD_ONLY
    )
  else()
    message(STATUS "Download asio from ${ASIO_REPOSITORY}:${ASIO_TAG}")
    FetchContent_Populate(asio)
  endif()

  # Configure ##

  add_library(asio STATIC src/asio.cpp)
  add_library(asio::asio ALIAS asio)
  target_include_directories(asio PUBLIC ${asio_SOURCE_DIR}/asio/include)

  target_compile_features(asio PUBLIC cxx_std_11)
  target_compile_definitions(asio PUBLIC
    -DASIO_STANDALONE
    -DASIO_SEPARATE_COMPILATION
  )

  # Fix warning : "Please define _WIN32_WINNT or _WIN32_WINDOWS appropriately."
  # https://stackoverflow.com/questions/9742003/platform-detection-in-cmake
  if (WIN32 AND CMAKE_SYSTEM_VERSION)
    set(ver ${CMAKE_SYSTEM_VERSION})
    string(REPLACE "." "" ver ${ver})
    string(REGEX REPLACE "([0-9])" "0\\1" ver ${ver})
    set(version "0x${ver}")
    target_compile_definitions(asio PUBLIC -D_WIN32_WINNT=${version})
  endif()

endif()
