# asio.cmake

[![👷 compilation](https://github.com/OlivierLDff/asio.cmake/actions/workflows/main.yml/badge.svg)](https://github.com/OlivierLDff/asio.cmake/actions/workflows/main.yml)

CMake wrapper to add [Asio](https://github.com/chriskohlhoff/asio) library.

## 🚀 How to use

### [FetchContent](https://cmake.org/cmake/help/latest/module/FetchContent.html)

```cmake
# Force to use FetchContent and not CPM to fetch Asio
option(ASIO_USE_CPM "Download Asio with CPM instead of FetchContent" ON)
option(
  ASIO_CPM_FETCHCONTENT_COMPAT
  "Should Asio be declared with FetchContent functions to be compatible. This doesn't not allow CPM cache to work"
  OFF
)

# Download
include(FetchContent)
FetchContent_Declare(
  asio.cmake
  GIT_REPOSITORY "https://github.com/OlivierLDff/asio.cmake"
  GIT_TAG        "1.3.0" # Update with latest release tag https://github.com/OlivierLDff/asio.cmake/releases/latest
)
FetchContent_MakeAvailable(asiocmake)

# ...

# Link library asio::asio or asio
target_link_libraries(myapp PRIVATE asio::asio)
```

### [CPM](https://github.com/cpm-cmake/CPM.cmake)

```cmake
include(cmake/get_cpm.cmake) # or include(cmake/CPM.cmake)

CPMAddPackage(
  NAME asio.cmake
  VERSION "1.3.0" # Update with latest release tag https://github.com/OlivierLDff/asio.cmake/releases/latest
  GITHUB_REPOSITORY "OlivierLDff/asio.cmake"
  EXCLUDE_FROM_ALL TRUE
  SYSTEM TRUE
)

# ...

# Link library asio::asio or asio
target_link_libraries(myapp PRIVATE asio::asio)
```

### Options

| **Name**                       | **Description**                                                                                                                                                                                           | **Type** | **Default**                             |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | --------------------------------------- |
| `ASIO_REPOSITORY`              | _Asio_ git repository url                                                                                                                                                                                 | `STRING` | `https://github.com/chriskohlhoff/asio` |
| `ASIO_TAG`                     | _Asio_ git tag                                                                                                                                                                                            | `STRING` | `asio-1-31-0`                           |
| `ASIO_USE_CPM`                 | Download _Asio_ with _CPM_ instead of _FetchContent_                                                                                                                                                      | `BOOL`   | `ON`                                    |
| `ASIO_CPM_FETCHCONTENT_COMPAT` | Should _Asio_ be declared with _FetchContent_ functions to be compatible. This does not allow _CPM_ cache to work. It is recommended to set it _OFF_ when using _CPM_, in order to use _CPM_SOURCE_CACHE_ | `BOOL`   | `OFF`                                   |
| `ASIO_NO_DEPRECATED`           | Disables _Asio_ deprecated interfaces and functionality                                                                                                                                                   | `BOOL`   | `ON`                                    |
| `ASIO_ENABLE_EXAMPLES`         | Add _Asio_ example to test binding/compilation                                                                                                                                                            | `BOOL`   | `PROJECT_IS_TOP_LEVEL`                  |
| `ASIO_ENABLE_INSTALL`          | Install _Asio_ standalone library                                                                                                                                                                         | `BOOL`   | `PROJECT_IS_TOP_LEVEL`                  |

## Asio

_Asio_ is a cross-platform C++ library for network and low-level I/O programming that provides developers with a consistent asynchronous model using a modern C++ approach.

### What does Asio provide?

_Asio_ provides the basic building blocks for C++ networking, concurrency and other kinds of I/O.

![asio_overview](https://think-async.com/Asio/overview.gif)

### What kind of applications can use Asio?

_Asio_ is used in all kinds of applications, from phone apps to the world’s fastest share markets.

Here is a list of [some applications that use Asio](https://think-async.com/Asio/WhoIsUsingAsio.html).

### Supported Platforms

_Asio_ is regularly tested with the following platforms:

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

This project is licensed under the [MIT](https://opensource.org/licenses/MIT) License. \
See [LICENSE](./LICENSE) file for details.

_Asio_ is released under the [Boost Software License](http://www.boost.org/LICENSE_1_0.txt).

## 👥 Authors

- [Olivier LDff](https://github.com/OlivierLDff/NetTcpJson/blob/main/olivier.ldff@gmail.com)
