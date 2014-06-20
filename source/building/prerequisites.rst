===============================
Prerequisites (all plaforms)
===============================

**C++11**

The core library is written in a lightweight subset of C++11. This makes the code more compact, easier to understand, and easier to maintain. 
You'll need a C++11 compiler like G++, Clang, or MSVC++.

**Standalone ASIO**

You'll need the header-only (non-BOOST) version of `ASIO <http://think-async.com/>` >= 1.10.2.  
Create an environment variable called ASIO_HOME and point it to your install location if it isn't installed into your build path automatically.

.. code-block:: bash

  ASIO_HOME = <path to 'include' directory of ASIO>

**Java (optional)**

If you want to build the java bindings shared library you need at least JDK 1.6. For either build system to find the jni headers you need to define the JAVA_HOME environment variable:

.. code-block:: bash

  JAVA_HOME = <root directory of the jdk installation>

On windows this might be something like:

**C:\\Program Files (x86)\\Java\\jdk1.xxx**

On linux this might be something like: 

**/usr/lib/jvm/java-7-oracle**
