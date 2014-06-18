
============================
Components
============================

Opendnp3 has a modular architecture to allow it to adapt to a number of platforms and languages. The following graph show the major modules (libraries and applications) that make up the opendnp3 project. Descriptions of each module are provided below the graph.

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
* Abstract executor (IExecutor) that provides a way to post events, get monotonic timestamps, and start/canel software timers

Openpal also provides buffer wrappers (read/write), statically-allocated container types, big/litte endian parsers for various integer/float types. Any opendnp3 program must have a concrete implementation of the PAL to execute. The library has no dependencies on STL, dynamic allocation, or the C++ standard library. It is suitable for deeply embedded systems.

**opendnp3**

This is the core dnp3 library providing implementations of both outstation and masters. It uses the interfaces provided in openpal, and is therefore platform independent. It has the same characteristics as openpal and can run on microcontroller platforms with a C++11 compiler.

**asiopal**

An implementation of the PAL for platforms with an operating systems. It uses ASIO to provide cross platform networking and serial port support. Each instance of IExecutor is on it's own "strand" allowing this PAL to transparently run via a thread pool. This is the component that gives opendnp3 such great scalability on multi-core systems.

