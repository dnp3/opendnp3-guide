.. _platforms-label:

=================================
Supported platforms
=================================

The core opendnp3 library is platform-neutral C++11.  It can target all major operating systems and a number of bare-metal microcontrollers.

**Operating Systems**

Operating system support is dependent on your C++11 compiler and the ASIO library support for your platform.

* Windows (MSVC++ & Visual Studio 2013 Express)
* Linux (GCC >= 4.7.3 or Clang >= 3.3)
* OSX (Clang >= 3.3)

**Embedded Linux**

The same Linux build has been used with a number of GCC-based ARM cross-compilers.
The example applications and unit tests will run on the Raspberry Pi and Beagle Bone Black.
These platforms are overkill, however, as the library will run on much more limited ARM-linux systems.

**Microcontrollers without an OS**

If you are willing to write a Platform Abstraction Layer (PAL, sometimes called a HAL elsewhere), opendnp3 might run on a microcontroller. Check out the *embedded* section for examples.
