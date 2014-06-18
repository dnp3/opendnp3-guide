
============================
Components
============================

Opendnp3 has a modular architecture to allow it to adapt to a number of platforms and languages. The following graph show the major modules that comprise the opendnp3 project. Descriptions of each module are provided below the graph.

**Related Topics**

.. toctree::  

  directories

**Key**

* **Arrows** represent a depency, i.e. "A depends on B" or "A requires B"
* **Plain-text** nodes represent toolchain requirements
* **Ellipses** represent a system or 3rd party library
* **Rectangles** represent a library in the opendnp3 project
* **Greyed** components can be used with microcontrollers without an OS
* **Diamonds** represent executable programs / firmware
* **Dashed** components are abstract, i.e. things you have to write yourself

.. graphviz:: modules.dot

**openpal**

PAL stands for Platform Abstraction Layer. It provides a number of abstract services that can be used to build protocol stacks in a platform independent fashion. There is nothing DNP3-specific about openpal. It could be used to build other protocol stacks. These services include:

* Abstract communication channels (IPhysicalLayer)
* Abstract executor (IExecutor) that provides a way to post events, get timestamps, and start/cancel software timers

Openpal also provides buffer wrappers (read/write), statically-allocated container types, big/litte endian parsers for various integer/float types. Any opendnp3 program must have a concrete implementation of the PAL to execute. The library has no dependencies on STL, dynamic allocation, or the C++ standard library. It is suitable for deeply embedded systems.

**opendnp3**

This is the core dnp3 library providing implementations of both outstation and masters. It uses the interfaces provided in openpal, and is therefore platform independent. It has the same characteristics as openpal and can run on microcontroller platforms with a C++11 compiler.

**asiopal**

An implementation of the PAL for platforms with an operating systems. It uses ASIO to provide cross platform networking and serial port support. Each instance of IExecutor is on it's own "strand" allowing this PAL to transparently run via a thread pool. This is the component that gives opendnp3 such great scalability on multi-core systems.

**asiodnp3**

This library presents a high-level interface to opendnp3 based on components of the asiopal. Almost every project running on an operating system should use the this library as its entry point. The C++ demos are built based on this library.

**.NET Bindings**

This is actually 2 libraries, a pure C# library of interfaces and a mixed-mode assemly written in C++/CLI. Together they comprise the .NET bindings and with an interface that roughly mirrors asiodnp3 but with more idiomatic .NET semantics.

**Java Bindings**

This is actually 2 libraries, a pure Java library (build with Maven) of interfaces and a JNI-based shared library that implements the native functions. Together they comprise the Java bindings and have an interface that roughly mirrors asiodnp3 but with more idiomatic Java semantics.

**Test Harness**

This is a standalone winforms application written in C#. It provides functionality similar to the test harnesses from TMW & ASE. It has a modular architecture that can be extended to write device simulators. It serves the dual purpose of providing a useful tool to the community and demoing the capabilities of the underlying library.


