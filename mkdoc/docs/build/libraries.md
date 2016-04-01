### Libraries

An overview of the directory structure is presented here. It describes only the most important elements, and is
not an exhaustive list of every folder in the project.

```sh
# C++ projects built using CMake

/cpp
  /libs
    /src
      /openpal       // interfaces, containers, and parsers (Platform Abstraction Layer)
	    /opendnp3      // the core dnp3 library w/ no external dependencies other than openpal
	    /asiopal       // provides ASIO-based implementations of things in openpal
	    /asiodnp3      // high-level dnp3 interface that leverage asiopal
      /dnp3decode    // a dnp3 decoder based on the opendnp3 parsers 

  /examples         // various example programs of using opendnp3
  /tests            // unit tests and shared helper libraries  
```

The dependencies between the libraries for linking purposes can be defined as:

**opendnp3 -> { openpal }**

**asiopal ->  { openpal }**

**asiodnp3 -> { opendnp3, asiopal, openpal }**

When building an external application on Linux for instance, you need to link against all four libraries:

```
-lasiodnp3 -lasiopal -lopendnp3 -lopenpal
```

### CLR Bindings

```sh
# CLR bindings built using external SLN

/dotnet
  /bindings
	/CLRInterface   // Pure CLR interfaces and classes that comprise the API
    /CLRAdapter		// a C++/CLI project that adapters the underlying C++ libraries to C#
```
