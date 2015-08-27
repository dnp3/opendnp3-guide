### Platforms

Opendnp3 is a cross-platform C++ libray. It targets all major operating systems including Linux, Windows, and OSX. In theory, it should work on 
any platform with a C++11 compiler that is also supported by [ASIO](http://think-async.com/Asio/asio-1.10.6/doc/asio/using.html).

There has been some experimentation with using parts of the library on bare-metal microcontrollers like AVR/ARM, but this is outside the scope
of this documentation.

### Compilers

The following compilers are known to work.

* MSVC++ (Ships w/ Visual Studio 2013)
* g++ >= 4.8.x 
* clang >= 3.3 

### C++11

C++11 introduced a standard library with lots of cross-platform goodies that had been platform-dependent for ages. The following C++11 items
are used in opendnp3.

* **std::thread** - platform-independent threading using to manage the thread pool
* **std::chrono** - platform-indepdent time operations including wall-clock and steady-clock used to manage timers

The final piece required to build a useful protocol stack is a cross-platform networking library.

### ASIO

[ASIO](http://think-async.com/) is a header-only library that provides asynchronous I/O (ASIO). Opendnp3 uses ASIO for several things.

* Cross-platform sockets
* Cross-platform serial ports
* A multi-core event loop
* Abstract timers on top of std::chrono






