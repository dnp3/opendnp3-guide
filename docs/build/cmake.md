### CMake

OpenDNP3 uses a build system generator called [CMake](http://www.cmake.org/).
This means that the actual build files (e.g. a Makefile or Visual Studio .SLN)
are generated from a artifact.

This allows the opendnp3 project to maintain a single project file for all
platforms, minimizing per-platform maintenance. CMake also integrates nicely
with C++ IDEs like [KDevelop](https://www.kdevelop.org/) or
[CLion](https://www.jetbrains.com/clion/).

### Optional Components

By default, CMake will not build tests, demos, or TLS support. You can enable
each optional component individually by specifying them on the command line:

```sh
> cmake -D<option>=ON
```

| Option Name    | Comments                                                   |
| -------------- | ---------------------------------------------------------- |
| DNP3_ALL       | build all optional components below                        |
| DNP3_DEMO      | build the example programs                                 |
| DNP3_DOTNET    | build the .NET bindings (Windows only)                     |
| DNP3_JAVA      | build the java bindings                                    |
| DNP3_TEST      | build the unit test suites                                 |
| DNP3_TLS       | build support for TLS channels (requires openssl >= 1.1.1) |
| DNP3_DECODER   | build the decoder module                                   |
| DNP3_FUZZING   | build Google OSS-Fuzz integration                          |

For example, to build the demos including TLS support:
```sh
> cmake -DDNP3_DEMO=ON -DDNP3_TLS=ON
```

### OpenSSL

When compiling with `DNP3_TLS` set, you must have OpenSSL >= 1.1.1 installed on
your system. The `libcrypto.dll|so` and `libssl.dll|so` must also be available
in your path.

On Windows, the easiest way is to download the precompiled binaries available
here at [ShiningLight](https://slproweb.com/products/Win32OpenSSL.html). For
building OpenDNP3, you will need the full version of the target architecture.
For simply executing an application, you can only download the "light" version.

On Unix systems, use the package manager to install the developer version of
OpenSSL. On Debian-based distribution, run `sudo apt-get install libssl-dev`
(you will need `buster` or higher).

### Build Options

Most of command-line options you can feed to CMake to generate your build
environment are platform-independent. This documentation can't tell you
everything that CMake can do. We only document some of the more common flags
here for your convenience. All of the following examples assume an out-of-source
build folder in a sibling directory to the opendnp3 distribution.

#### Static vs Dynamic Linking

You can switch between building static or dynamic linking using the
`DNP3_STATIC_LIBS` flag. Note that this flag is provided by the project and is
not a CMake flag.

On Windows, static libs are the default. On Linux, dynamic libs are the default.

```
> cmake DNP3_STATIC_LIBS=ON   # build static libraries
> cmake DNP3_STATIC_LIBS=OFF  # build dynamic libraries
```

#### Debug vs Release

You can configure release vs debug builds using the CMAKE_BUILD_TYPE flag. Note
that on Windows, the generated SLN contains debug and release build targets
already
```sh
> cmake -DCMAKE_BUILD_TYPE=Debug
> cmake -DCMAKE_BUILD_TYPE=Release
```

#### Non-default generators

By default, CMake will pick a generator to use if you don't tell it which one.
You can see a list of all available generators using the help flag.
```sh
> cmake --help
```
You can then specify a specific generator, e.g. to do a full 64-bit build on
Windows:

```sh
> mkdir build64
> cmake .. -DDNP3_ALL=ON -G "Visual Studio 14 2015 Win64"
```

#### Setting the install prefix

If the default install location isn't where you want it to be, you can configure
it using `CMAKE_INSTALL_PREFIX`.

On Debian-based systems this should probably be:
```sh
> cmake .. -DCMAKE_INSTALL_PREFIX=/usr/lib
```

On windows, you might put your libraries and headers somewhere like:
```sh
> cmake .. -DCMAKE_INSTALL_PREFIX=C:\libs\opendnp3
```

