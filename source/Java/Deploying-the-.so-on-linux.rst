Deploying the .so on linux
--------------------------

Building and installing the shared library on Linux is a snap. You use the same tool-chain you used for the c++ library, but instead you run configure with a flag that adds additional make support.

.. code-block:: bash

   $ ./configure --with-java=javac
   $ make
   $ sudo make install


Two things to note. You can specify _which_ java, since a macro in configure looks for your jdk directory based on the location of javac. Secondly, the shared library libopendnp3java.so depends on the presence of the core C++ library libopendnp3.so. You must have both of these installed as loading the java library will automatically load the other since they're libtool libraries.

The best way to verify that you've installed the library correctly is to run the maven test suite by invoking:

.. code-block:: bash

   $ cd java
   $ mvn test
