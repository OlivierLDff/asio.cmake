# Asio.cmake

CMake wrapper to add [asio](https://github.com/chriskohlhoff/asio) with a simple FetchContent as a static library.

## 🚀 How to use

Use `FetchContent` to add this repository in your cmake script.

```cmake
# Download this repository
include(FetchContent)
FetchContent_Declare(
  asiocmake
  GIT_REPOSITORY "https://github.com/OlivierLDff/asio.cmake"
  GIT_TAG        "master"
)
FetchContent_MakeAvailable(asiocmake)

# ...

# Link to the created library asio::asio or asio
target_link_libraries(myapp PRIVATE asio::asio)
```

### Customization

* `ASIO_REPOSITORY`: From where asio will be downloaded. *Default `https://github.com/chriskohlhoff/asio`*
* `ASIO_TAG`: Tag of asio that will be used. *Default: `master`*

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

## 👥 Authors

- [Olivier LDff](https://github.com/OlivierLDff/NetTcpJson/blob/main/olivier.ldff@gmail.com)
