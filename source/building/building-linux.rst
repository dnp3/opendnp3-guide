===================
Building on Linux
===================

The build process on Linux should be very familiar.

The default prefix is set for Ubuntu or Fedora (/usr) so you may need to adjust this for another Linux distribution so that headers and libraries get sent to the right place.

To build and install the libraries and demo applications:

.. code-block:: bash

   $ autoreconf -i -f 
   $ ./configure
   $ make -j <cores>
   $ sudo make install

To build the unit tests:

.. code-block:: bash
  
   $ make check

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

