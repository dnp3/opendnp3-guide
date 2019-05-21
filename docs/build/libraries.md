### Libraries

An overview of the directory structure is presented here. It describes only the most important elements, and is
not an exhaustive list of every folder in the project.

```bash
# C++ projects built using CMake

/cpp
  /libs
    /include
      /openpal       // interfaces, containers, and parsers (Platform Abstraction Layer)
      /opendnp3      // the core dnp3 library w/ no external dependencies other than openpal
      /asiopal       // provides ASIO-based implementations of things in openpal
      /asiodnp3      // high-level dnp3 interface that leverage asiopal
      /dnp3decode    // a dnp3 decoder based on the opendnp3 parsers
```

The dependencies between the libraries for linking purposes can be defined as:

**`opendnp3 -> { openpal }`**

**`asiopal ->  { openpal }`**

**`asiodnp3 -> { opendnp3, asiopal, openpal }`**

When building an external application on Linux for instance, you need to link against all four libraries:

```
-lasiodnp3 -lasiopal -lopendnp3 -lopenpal
```

### CLR Bindings

```sh
# CLR bindings built using external SLN

/dotnet 
  /CLRInterface   // Pure CLR interfaces and classes that comprise the API
  /CLRAdapter     // a C++/CLI project that adapters the underlying C++ libraries to C#
  /examples       // C# example applications
```

### Java Bindings

```sh
# Java bindings built using CMake (native) and Maven (pom.xml)

/java  
  /bindings       // Java w/ native stubs
  /codegen        // Code generator written in Scala that generates C++ JNI boilerplate
  /cpp            // C++ compiled to libopendnp3java.dll/so
  /example        // Java example programs that depend on api/bindings Jars
```
