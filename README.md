# Asio.cmake

[![👷 compilation](https://github.com/OlivierLDff/asio.cmake/actions/workflows/main.yml/badge.svg)](https://github.com/OlivierLDff/asio.cmake/actions/workflows/main.yml)

CMake wrapper to add [asio](https://github.com/chriskohlhoff/asio) with a simple FetchContent as a static library.

## 🚀 How to use

### [FetchContent](https://cmake.org/cmake/help/latest/module/FetchContent.html)

Use `FetchContent` to add this repository in your cmake script.

```cmake
# Force the script to use FetchContent and not CPM internally to fetch asio
option(ASIO_USE_CPM "Download Asio with CPM instead of FetchContent" OFF)
option(
  ASIO_CPM_FETCHCONTENT_COMPAT
  "Should asio be declared with FetchContent functions to be compatible. This doesn't not allow CPM cache to work."
  ON
)
# Download this repository
include(FetchContent)
FetchContent_Declare(
  asiocmake
  GIT_REPOSITORY "https://github.com/OlivierLDff/asio.cmake"
  GIT_TAG        "main"
)
FetchContent_MakeAvailable(asiocmake)

# ...

# Link to the created library asio::asio or asio
target_link_libraries(myapp PRIVATE asio::asio)
```

### [CPM](https://github.com/cpm-cmake/CPM.cmake)

Or with CPM even an easier integration.

```cmake
include(cmake/CPM.cmake)

CPMAddPackage(
  NAME asiocmake
  GIT_REPOSITORY "https://github.com/OlivierLDff/asio.cmake"
  GIT_TAG "main"
)

# ...

# Link to the created library asio::asio or asio
target_link_libraries(myapp PRIVATE asio::asio)
```

### Customization

* `ASIO_REPOSITORY`: From where asio will be downloaded. *Default `https://github.com/chriskohlhoff/asio`*
* `ASIO_TAG`: Tag of asio that will be used. *Default: `master`*
* `ASIO_USE_CPM`: Download Asio with CPM instead of FetchContent. *Default: `OFF`*
* `ASIO_SEPARATE_COMPILATION`: Uses separately compiled source code for Asio's implementation. *Default: `ON`*
* `ASIO_DYN_LINK`: Uses separately compiled source code for Asio's implementation, with symbols exported for inclusion as part of a shared library. *Note*: Available only if `ASIO_SEPARATE_COMPILATION` is `ON`. *Default: `OFF`*
* `ASIO_NO_DEPRECATED`: Disables _Asio_ deprecated interfaces and functionality. *Default: `ON`*
* `ASIO_CPM_FETCHCONTENT_COMPAT`: Stay compatible in a build that use FetchContent when using CPM. *Default `ON`*
  * It is recommended to set it OFF when using CPM, in order to use `CPM_SOURCE_CACHE`

* `ASIO_ENABLE_EXAMPLES`: Add asio examples. *Default: `PROJECT_IS_TOP_LEVEL`*
* `ASIO_ENABLE_INSTALL`: Install asio target. *Default: `PROJECT_IS_TOP_LEVEL`*

## Asio

Asio is a cross-platform C++ library for network and low-level I/O programming that provides developers with a consistent asynchronous model using a modern C++ approach.

### What does Asio provide?

Asio provides the basic building blocks for C++ networking, concurrency and other kinds of I/O.

![](https://think-async.com/Asio/overview.gif)

### What kind of applications can use Asio?

Asio is used in all kinds of applications, from phone apps to the world’s fastest share markets.

Here is a list of [some applications that use Asio](https://think-async.com/Asio/WhoIsUsingAsio.html).

### Supported Platforms

Asio is regularly tested with the following platforms:

- Linux
  - gcc 4.8 or later
  - clang 3.0 or later
- macOS
  - Xcode8 or later using bundled clang
- Windows
  - Visual C++ 2013 or later
- FreeBSD
  - gcc 6.0 or later

[More…](https://think-async.com/Asio/SupportedPlatforms.html)

## 📄 License

```
MIT License

Copyright (c) 2021 Olivier Le Doeuff

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

Asio is released under the [Boost Software License](http://www.boost.org/LICENSE_1_0.txt).

## 👥 Authors

- [Olivier LDff](https://github.com/OlivierLDff/NetTcpJson/blob/main/olivier.ldff@gmail.com)
