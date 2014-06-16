========================
Prerequisites on Linux
========================

**g++ 4.6.x**

The reason the compiler support required is so cutting edge is because of C++11. This was done primarily to reduce the required pieces of Boost. Installation may vary from platform to platform.


**Ubuntu 12.04**


.. code-block:: bash

   $ sudo apt-get install g++


**Fedora18**


Use the package manager to install gcc and gcc-c++ or:-

.. code-block:: bash

   $ sudo yum install gcc gcc-c++

Will install or update the compilers or indicate that they are already installed and latest version.

**GNU autotools**

Most developers already have this installed, but here are some platform specific hints.

**Ubuntu 12.04**


.. code-block:: bash

   $ sudo apt-get install autoconf libtool

**Fedora18**

Use the package manager to install autoconf, m4, libtool and automake, or:-

.. code-block:: bash

   $ sudo yum install autoconf m4 libtool automake

**Boost**

Most linux distros have packages for at least 1.50.0.

.. code-block:: bash

   $ sudo apt-get install liboost-1.50-all-dev

**Fedora18**

Use the package manager to install boost, or:-

.. code-block:: bash

   $ sudo yum install boost


If you want to install the latest and greatest you need to build from source but its pretty easy:

.. code-block:: bash

   $ wget http://sourceforge.net/projects/boost/files/boost/1.52.0/boost_1_52_0.tar.gz/download -O boost.tar.gz
   $ tar -xvf boost.tar.gz
   $ cd boost_1_52_0
   $ ./bootstrap.sh
   $ sudo ./b2 install --prefix=/usr --with-system --with-test


If you just want to go ahead and install ALL of boost in case you want it for something else in the future just leave off the '--with-' statements. The prefix given may vary for another Linux disto.

