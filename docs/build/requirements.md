### Platforms

OpenDNP3 is a cross-platform C++ library. It is tested on Linux and Windows.

### C++14

OpenDNP3 uses the following C++11/14 features:

* **`std::unique_ptr`** & **`std::shared_ptr`** (including
  **`std::make_unique`**, part of C++14) - smart pointers are used for automatic
  memory management
* **`std::thread`** - platform-independent threading used to manage the thread
  pool
* **`std::chrono`** - platform-independent time operations including
  steady-clock used to manage timers
* **`variadic templates`** - used to simplify parsing routines.

### CMake

[CMake](http://www.cmake.org/) is a cross-platform tool to build and test C++
software. OpenDNP3 requires CMake 3.11 or higher to build.

During the build process, it will automatically download or locate the
dependencies required to build the library.

### ASIO

[ASIO](https://think-async.com/Asio/) is a header-only library that provides
asynchronous I/O (ASIO). OpenDNP3 uses ASIO for several things.

* Cross-platform sockets
* Cross-platform serial ports
* A multi-core event loop
* Abstract timers on top of `std::chrono`

This dependency is automatically fetched by CMake on compilation.

### Compilers

The following compilers are known to work, and are tested frequently.

* MSVC++ - Visual Studio >= 2019
* g++ >= 4.9.x
* clang >= 3.5
