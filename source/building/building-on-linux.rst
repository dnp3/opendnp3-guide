===================
Building on Linux
===================

The build process on Linux should be very familiar. To start either clone the git repository or download an unpack a tarball from the landing page.

The default prefix is set for Ubuntu or Fedora (/usr) so you may need to adjust this for another Linux distribution so that headers and libraries get sent to the right place.

To build the libraries and install them + the headers:

.. code-block:: bash

   $ autoreconf -i -f 
   $ ./configure
   $ make -j <parallelism, 1.5x times number of cores/cpus>
   $ sudo make install


If you want to run the unit tests, you can type

.. code-block:: bash
  
   $ make check


This will produce and run an executable called dnp3test that uses boost::unit_test_framework command line arguments. Extensive documentation on this can be found [here](http://www.boost.org/doc/libs/1_53_0/libs/test/doc/html/index.html), but a good default invocation so that you can see a progress bar is:

.. code-block:: bash

   $ ./dnp3test --show_progress


Once you have the library installed, you may also want to check out the demo programs. They use a separate autotools instance in cpp/demos. The reason this is done this way is so that during development the demos can be used a test that the main toolchain is properly installing the right libraries and headers.

=======================
Code coverage with GCC
=======================

.. code-block:: bash

   ./configure CXXFLAGS="--coverage -g -O0" LDFLAGS="--coverage -g -O0"


.. code-block:: bash

   make check
   ./dnp3test
   ./openpaltest


now generate the info file with lcov

.. code-block:: bash

   lcov -c -d ./ -b ./ -o coverage.info


and then the html versions:
   
.. code-block:: bash

   genhtml coverage.info -o test_html

