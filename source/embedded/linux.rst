
==================
Embedded Linux
==================

Platforms like the Raspberry Pi and Beagle Bone Black allow you to compile directly on the target (slowly). For other platforms, you'll need a cross compiler for your particular hardware that's up to date with C++11 support. Sub-bullets of this topic address embedded issues like memory usage and library size.

There's nothing special for targeting embedded Linux. You still need ASIO and you should use the *asiodnp3* library and examples as your reference point.

If you're unfamiliar with cross compiling in general, I'd recommend getting an embedded Linux book. 

**Raspberry Pi**

The `Raspberry Pi <http://www.raspberrypi.org/>`_ is a great platform to start learning about cross-compiling. You can compile Opendnp3 on the Pi itself, but since it's a fairly large library, you'll probably want to cross-compile.

Lots of different tool-chains will work, but `this guide <http://www.kitware.com/blog/home/post/426>`_ can show you how to build one yourself using crosstool-ng. It can build crosstool chains for a variety of platforms like ARM, powerPC, etc. After you have a working toolchain, test it with some trivial example apps before proceeding with cross-compiling Opendnp3.

.. code-block:: bash

   env CPPFLAGS="-I/home/jadamcrain/x-tools/arm-unknown-linux-gnueabi/include" ./configure --host=arm-unknown-linux-gnueabi 
   --build=i686-pc-linux-gnu --with-boost-libdir=/home/jadamcrain/x-tools/arm-unknown-linux-gnueabi/lib LDFLAGS="-lpthread" CXXFLAGS=-Os


Before trying to write an applications using Opendnp3 for your linux embedded system, make sure the test suite passes first. The test suite runs some pretty aggressive integration tests, so be patient with the little Pi. For less capable systems, you may want to turn down some of the integration testing.

.. code-block:: bash

   pi@raspberrypi ~ $ ./dnp3test --show_progress

   0%   10   20   30   40   50   60   70   80   90   100%
   |----|----|----|----|----|----|----|----|----|----|
   Running 458 test cases...
   ***************************************************

   *** No errors detected
