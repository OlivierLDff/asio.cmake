# Changelog

## v1.2.0 - 2022/09/06

* Use CPM and drop FetchContent compatibility by default
* ASIO_ENABLE_INSTALL cmake option to install asio
* add `ASIO_NO_DEPRECATED` public flag
* Update minimal version of CMake to 3.21.0 to use new `PROJECT_IS_TOP_LEVEL`

## v1.1.3 - 2022/09/01

* ASIO_CPM_FETCHCONTENT_COMPATASIO_CPM_FETCHCONTENT_COMPAT can be set to OFF to use CPM_SOURCE_CACHE and have a fast cmake configure step

## v1.1.2 - 2022/09/01

* asio-1-24-0
* CPM v0.35.5
* Ci test arm

## v1.1.1 - 2021/08/24

* ASIO_USE_CPM to download via CPM
* Fix "Please define _WIN32_WINNT or _WIN32_WINDOWS appropriately."

## v1.0.0 - 2021/06/14

* First release with `FetchContent` support
