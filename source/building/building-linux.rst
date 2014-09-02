===================
Linux Build
===================

The build process on Linux follows the standard autotools conventions. The project does not redistribute the configure script, so you need to run autoreconf as described below.

**GNU autotools**

You'll need g++, autoconf, and libtool

Don't forget to define the ASIO_HOME environment variable.

The default prefix is set for Ubuntu or Fedora (/usr) so you may need to adjust this for another Linux distribution so that headers and libraries get sent to the right place.

To build and install the libraries and demo applications:

.. code-block:: bash

   $ autoreconf -i -f 
   $ ./configure
   $ make -j <cores>
   $ sudo make install

To build and run the unit tests:

.. code-block:: bash
  
   $ make check -j <cores>
   $ ./dnp3test
   $ ./openpaltest

**Configuring for Clang**

Using Clang  is identical, except with the following argument in the configure step.

.. code-block:: bash

  $ ./configure CXX='clang++' CXXFLAGS='-std=c++11 -stdlib=libc++'

**Code coverage with GCC/GCOV/LCOV**

.. code-block:: bash

   $ ./configure CXXFLAGS="--coverage -g -O0" LDFLAGS="--coverage -g -O0"


.. code-block:: bash

   $ make check
   $ ./dnp3test
   $ ./openpaltest


now generate the info file with lcov

.. code-block:: bash

   $ lcov -c -d ./ -b ./ -o coverage.info


and then the html versions:
   
.. code-block:: bash

   $ genhtml coverage.info -o test_html

