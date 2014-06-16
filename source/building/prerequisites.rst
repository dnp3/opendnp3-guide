==========================
Common Prerequisites 
==========================

**C++11**

The core library written in C++, but makes use of lightweight C++11 features. This makes the code faster and easier to understand, but must have a C++11 compiler.

**Boost libraries**

The usage of boost has been substantially reduced from the original open source project. The main library only needs Boost::ASIO (header-only) and Boost::System (library). Follow the platforms specific directions to get Boost installed on your system. An asio-like networking api may be available in future releases of the C++ standard at which point the project may drop Boost altogether.

**Boost libraries >= version 1.50.0**

This is the default. To achieve steady timer support, it uses the boost::asio::steady_timer typedef added in 1.50.0.  You don't have to do anything special to use this.

Avoid Boost::ASIO 1.54 for Linux. There's a `regression <https://svn.boost.org/trac/boost/ticket/8795>`_ that will cause you grief

**Boost libraries < version 1.50.0**

The pre-processor directive OPENDNP3_BOOST_TIMER_PATCH must be defined to use BOOST versions prior to 1.50.0. Using autotools, this looks like:

.. code-block:: bash

  $ ./configure CXXFLAGS=-DOPENDNP3_BOOST_TIMER_PATCH


**Platforms besides Windows and Linux**

In theory, the library should work on MacOS and a host of other platforms supported by BOOST, but no one has tried it yet. Be the first!

**Java (optional)**

If you want to build the java bindings shared library you need at least JDK 1.6. For either build system to find the jni headers you need to define the JAVA_HOME environment variable:

.. code-block:: bash

  JAVA_HOME = <root directory of the jdk installation>

On windows this might be something like:

**C:\\Program Files (x86)\\Java\\jdk1.xxx**

On linux this might be something like: 

**/usr/lib/jvm/java-7-oracle**
