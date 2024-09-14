# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.2.1](https://github.com/OlivierLDff/asio.cmake/releases/tag/v1.2.1) - 2024-04-19

### Changed

- Minimum _CMake_ version set to `3.14.0`

## [1.2.0](https://github.com/OlivierLDff/asio.cmake/releases/tag/v1.2.0) - 2022-09-06

### Added

- Option `ASIO_ENABLE_INSTALL` to install _Asio_
- Compile definition `ASIO_NO_DEPRECATED`
- CMake `PROJECT_IS_TOP_LEVEL` to determine if subproject (using add_subdirectory) or if main project

### Changed

- Minimum _CMake_ version set to `3.21.0`

## [1.1.3](https://github.com/OlivierLDff/asio.cmake/releases/tag/v1.1.3) - 2022-09-01

### Added

- Option `ASIO_CPM_FETCHCONTENT_COMPAT` if `Asio` should be declared with _FetchContent_ functions to be compatible. `ASIO_CPM_FETCHCONTENT_COMPAT` can be set to `OFF` to use `CPM_SOURCE_CACHE` and have a fast cmake configure step

## [1.1.2](https://github.com/OlivierLDff/asio.cmake/releases/tag/v1.1.2) - 2022-09-01

### Added

- _arm_ architecture in CI

### Changed

- Default _Asio_ tag set to `asio-1-24-0`
- Updated _CPM_ to `v0.35.5`

## [1.1.1](https://github.com/OlivierLDff/asio.cmake/releases/tag/v1.1.1) - 2021-08-24

### Added

- _CPM_ usage
- Part of _Asio_ README
- [cmake format](https://github.com/cheshirekow/cmake_format)
- Timer example to check linkage

### Security

- Fetch fork _CPM_

### Fixed

- Find and link _Threads_

## [1.1.0](https://github.com/OlivierLDff/asio.cmake/releases/tag/v1.1.0) - 2021-08-07

### Added

- Option `ASIO_USE_CPM` to download via _CPM_
- Test compilation o _Windows_, _Mac_, and _Linux_

### Fixed

- Warning message _"Please define \_WIN32_WINNT or \_WIN32_WINDOWS appropriately."_

## [1.0.0](https://github.com/OlivierLDff/asio.cmake/releases/tag/v1.0.0) - 2021-06-14

### Added

- First release with `FetchContent` support
