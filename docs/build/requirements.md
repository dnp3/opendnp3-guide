### Platforms

Opendnp3 is a cross-platform C++ library. It targets all major operating systems including Linux, Windows, and OSX.

### C++14

Opendnp3 uses the following C++11/14 features:

* **std::unique_ptr / std::shared_ptr** - smart pointers are used for automatic memory management
* **std::thread** - platform-independent threading using to manage the thread pool
* **std::chrono** - platform-independent time operations including steady-clock used to manage timers
* **variadic templates** - used in to simplify parsing routines.

### ASIO

[ASIO](http://think-async.com/) is a header-only library that provides asynchronous I/O (ASIO). Opendnp3 uses ASIO for several things.

* Cross-platform sockets
* Cross-platform serial ports
* A multi-core event loop
* Abstract timers on top of std::chrono

### Compilers

The following compilers are known to work, and are tested frequently.

* MSVC++ - Visual Studio 2015 and above
* g++ >= 4.9.x
* clang >= 3.5
