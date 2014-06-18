
============================
Project Components & Layout
============================

Opendnp3 has a modular architecture to allow it to adapt to a number of platforms and languages.
The following graph show the major modules (libraries and applications) that make up the opendnp3 project.

* **Arrows** represent a depency, i.e. "A depends on B" or "A requires B"
* **Plain-text** nodes represent toolchain requirements
* **Ellipses** represent a system or 3rd party library
* **Rectangles** represent a library in the opendnp3 project
* **Greyed** components can be used with microcontrollers without an OS
* **Diamonds** represent executable programs / firmware
* **Dashed** components are abstract, i.e. things you have to write yourself

.. graphviz:: modules.dot

**The main directories are organized by language**

.. code-block:: java

   /cpp                     // root for all c++ source code
     /include/opendnp3      // public headers
     /src/opendnp3          // private part of the core library and implementation
     /tests                 // tests of the core library
     /demos                 // simple master and outstation example projects
   /java                    // root for Java bindings and Maven pom.xml
     /cpp                   // where JNI bindings and adapters live
     /api                   // java-doced api in pure java
     /bindings              // classes with native methods and adapter code
     /example               // java master / outstation examples
   /clr                     // Microsoft .NET wrappers
     /DNP3CLRInterface      // pure C# interfaces
     /DNP3CLRAdapter        // C++ CLR adapter code
     /DNP3CLRMasterDemo     // example master application in C#
     /DNP3CLROutstationDemo // example outstation application in C#


**Minor directories**

.. code-block:: java

   /config                  // visual studio property sheets mostly
   /scripts                 // install scripts for Boost if you MUST
   /m4                      // autotools macros
   /profile                 // device profile documents (not up-to-date)

