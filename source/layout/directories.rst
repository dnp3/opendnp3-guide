
============================
Diretory structure
============================

**The main directories are organized by language and then by category**

.. code-block:: java

   /cpp                     // root for all C++ source code
     /libs                  
       /openpal             // library of interfaces for the PAL, data structures, parsers
       /opendnp3            // core dnp3 library
       /asiopal             // implementation of the PAL using ASIO
       /asiodnp3            // dnp3 wrapper library for operating systems
     /examples              
       /master              // demo master application
       /outstation          // demo outstation application
     /tests                 
       /openpaltests        // tests cases for openpal
       /opendnp3tests       // test cases for opendnp3, asiopal, and asiodnp3
       /apdufuzzer          // an 'in-memory' fuzzer for the APDU parser
       /catch               // a redistribution of the Catch testing library

   /dotnet                  // Microsoft .NET wrappers and applications
     /bindings
       /CLRInterface        // pure C# interfaces for dnp3 (assembly)
       /CLRAdapter          // C++/CLI adapter code (mixed-mode assembly)
     /examples
       /master              // example master application in C#
       /outstation          // example outstation application in C#
     /simulator             
       /Simulator           // Graphical Winform DNP3 Test Harness
       /SimulatorAPI        // API for writing simulator modules

**other directories**

.. code-block:: java

   /config                  // visual studio property sheets, astyle config, doxygen config   
   /m4                      // autotools macros
   /generation              // code generator for app objects, parsers, and enums (C++/C#/Java)
   /profile                 // device profile document (needs work)

