### CMake

Opendnp3 uses a build system generator called [CMake](http://www.cmake.org/).  This means that the actual build files (e.g. a Makefile or Microsfot .SLN) are generated from a
common artifact called **CMakeLists.txt**. You can see what one looks like [here](https://github.com/automatak/dnp3/blob/2.0.x/CMakeLists.txt).

This allows the opendnp3 project to maintain a build file for all platforms, and greatly reduces per-platform maintainence. CMake also integrates nicely with
Linux C++ IDEs like [KDevelop](https://www.kdevelop.org/) or [CLion](https://www.jetbrains.com/clion/).

One of the more attractive parts of CMake is that it supports [out-of-source builds](http://www.cmake.org/Wiki/CMake_FAQ#Out-of-source_build_trees).

### Locating ASIO

The include sub-folder of the ASIO distribution (the folder that contains 'asio.hpp') needs to be on your include path, but there are several ways you can do this. 
You can choose the option that makes the most sense your particular build environment. CMake will try the following things in order to  locate your ASIO headers. 

**Look to see if the variable ASIO_HOME was deinfed via the cmake command line. If it is, the value will be added as an include path to the compiler.**

```sh
> cmake ../dnp3 -DASIO_HOME=C:\libs\asio-asio-1-10-6\asio\include
```
**Otherwise, it will check to see if it is defined as an environment variable. If it is, the value will be added as an include path to the compiler**

For instance, on Windows you might define your enviroment variable to look like this.
```sh
ASIO_HOME=C:\libs\asio-asio-1-10-6\asio\include
```
On Ubuntu Linux, you might add a line to ~/.bashrc as follows:
```sh
export ASIO_HOME=~/asio-asio-1-10-6/asio/include
```

**Lastly, cmake will just assume the headers are installed on the system. No checks are performed, so the build will fail is this isn't true**

### Optional Components

By default, cmake will not build tests, demos, TLS support, or the experimental secure authentication extensions. You can enable each optional component individually by specifying
them on the command line:

```sh
> cmake ../dnp3 -D<option>=ON
```

| Option Name    | Comments                                          |
| -------------- | ------------------------------------------------- |
| DEMO           | build the example programs                        |
| TEST           | build the unit test suites                        |
| DNP3_TLS       | build support for TLS channels (requires openssl) |
| SECAUTH        | build support for SA (requires openssl)           |
| FULL           | build ALL optional components                     |



For example, to build the demos including TLS support:
```sh
> cmake ../dnp3 -DDEMO=ON -DDNP3_TLS=ON
```

### Build Options

Most of command-line options you can feed to CMake to generate your build environment are platform-independent.  This documentation can't tell you everything that
CMake can do. We only document some of the more common flags here for your convenience. All of the following examples assume an out-of-source build folder in a 
sibling directory to the opendnp3 distribution.

**Static vs Dynamic Linking**

You can switch between building static or dynamic linking using the STATICLIBS flag. Note that this flag is provided by the project and is not a CMake flag.

On Windows, static libs are the default. On Linux, dynamic libs are the default.

```
> cmake ../dnp3 STATICLIBS=ON	# build static libraries
> cmake ../dnp3 STATICLIBS=OFF	# build dynamic libraries
```

**Debug vs Release**

You can configure release vs debug builds using the CMAKE_BUILD_TYPE flag
Note that on windows, the generated SLN contains debug and release build targets already
```sh
> cmake ../dnp3 -DCMAKE_BUILD_TYPE=Debug
> cmake ../dnp3 -DCMAKE_BUILD_TYPE=Release
```

**Non-default generators**

By default, CMake will pick a generator to use if you don't tell it which one. You can see a list of all available generators using the help flag.
```sh
> cmake --help
```
You can then specify a specific generator, i.e.
```sh
> cmake ../dnp3 -G "Visual Studio 12 2013"
```

**Setting the install prefix**

The default install prefix probably won't be right for your platform. You can configure it using CMAKE_INSTALL_PREFIX.

On Debian-based systems this should probably be:
```sh
> cmake ../dnp3 -DCMAKE_INSTALL_PREFIX=/usr
```

On windows, you might put your libraries and headers somewhere like:
```sh
> cmake ../dnp3 -DCMAKE_INSTALL_PREFIX=C:\libs\opendnp3
```

### Building on Linux

On Linux, the easiest way to use CMake is just to let it create a makefile for you. You can then use this makefile in the same way you normally would.
```sh
> cmake ../dnp3 <options>
> make
> sudo make install
```