Project layout
==============

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

