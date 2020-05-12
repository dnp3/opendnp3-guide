
The core C++ library lives in `cpp/lib/`. Public headers are in `cpp/lib/include/opendnp3`.

The library has no public external dependencies. ASIO is completely hidden from the public API.

When building an external application on Linux without TLS support, you need only need to link against opendnp3:

```
-lopendnp3
```

### CLR Bindings

The .NET bindings live in the `dotnet` folder. CMake can optionally generate the projects for them as part of the
SLN file during build configuration.

```sh
# CLR bindings built using CMake

/dotnet
  /CLRAdapter     // a C++/CLI project that adapts the underlying C++ libraries to C# 
  /CLRInterface   // Pure CLR interfaces and classes that comprise the API  
  /examples       // C# example applications
```

### Java Bindings

The java bindings live in the `java` folder. CMake can optionally generate the projects for them as part of the
SLN or makefile during build configuration.

```sh
# Java bindings built using CMake (native) and Maven (pom.xml)

/java  
  /bindings       // Java w/ native stubs
  /codegen        // Code generator written in Scala that generates C++ JNI boilerplate
  /cpp            // C++ compiled to libopendnp3java.dll/so
  /example        // Java example programs that depend on api/bindings Jars
```
