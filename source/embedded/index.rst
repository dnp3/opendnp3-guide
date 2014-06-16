Embedded systems
================

OpenDNP3 can be run on a variety of embedded systems. You'll need a cross compiler for your particular hardware that's up to date with C++0x support. Sub-bullets of this topic address embedded issues like memory usage and library size.

If you're unfamiliar with cross compiling in general, I'd recommend getting an embedded Linux book. 

**Raspberry Pi**

The `Raspberry Pi <http://www.raspberrypi.org/>`_ is a great platform to start learning about cross-compiling. You can compile OpenDNP3 on the Pi itself, but since it's a fairly large library, you'll probably want to cross-compile.

Lots of different tool-chains will work, but `this guide <http://www.kitware.com/blog/home/post/426>`_ can show you how to build one yourself using crosstool-ng. It can build crosstool chains for a variety of platforms like ARM, powerPC, etc. After you have a working toolchain, test it with some trivial example apps before proceeding with cross-compiling OpenDNP3.

The first step is to build boost for the Pi. Follow the directions `here <http://www.boost.org/boost-build2/doc/html/bbv2/tasks/crosscompile.html>`_.  Remember, you only need Boost::System and Boost::Test.

With ARM libraries for Boost, you're ready to use autotools to cross-compile. My invocation of configure looked like:

.. code-block:: bash

   env CPPFLAGS="-I/home/jadamcrain/x-tools/arm-unknown-linux-gnueabi/include" ./configure --host=arm-unknown-linux-gnueabi 
   --build=i686-pc-linux-gnu --with-boost-libdir=/home/jadamcrain/x-tools/arm-unknown-linux-gnueabi/lib LDFLAGS="-lpthread" CXXFLAGS=-Os


You need to then copy your compiled Boost libraries and OpenDNP3 over to the PI using SCP or Filezilla (I personally prefer this). 

.. code-block:: bash

   pi@raspberrypi ~ $ ls /usr/lib/libboost_*
   /usr//lib/libboost_prg_exec_monitor.so.1.53.0  /usr//lib/libboost_system.so.1.53.0  /usr//lib/libboost_unit_test_framework.so.1.53.0
   pi@raspberrypi ~ $ ls /usr/lib/libopendnp3*
   /usr/lib/libopendnp3.so.1.0.1


Before trying to write an applications using OpenDNP3 for your embedded system, make sure the test suite passes first. The test suite runs some pretty aggressive integration tests, so be patient with the little Pi. For less capable systems, you may want to turn down some of the integration testing.

.. code-block:: bash

   pi@raspberrypi ~ $ ./dnp3test --show_progress

   0%   10   20   30   40   50   60   70   80   90   100%
   |----|----|----|----|----|----|----|----|----|----|
   Running 458 test cases...
   ***************************************************

   *** No errors detected

