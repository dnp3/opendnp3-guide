### Visual Studio SLN

On windows, CMake is used to generate a SLN and associated project files.

```sh
> mkdir build
> cd build
> cmake .. <options>
```

Once you've generated your SLN, you can just open it and use Visual Studio to build the project for you just like any other project.

### Command Line

Alternatively, you can just invoke msbuild.exe on the generated solution and build from the command line.

```sh
> msbuild opendnp3.sln
```

### Installing

CMake creates a special project called "install" that you can run inside Visual Studio to install the headers and libraries to
the directory specified by CMAKE_INSTALL_PREFIX.

### TLS Support

If you need to build the stack w/ TLS support (or you're using the .NET bindings), then you need to install openssl on Windows.  Use the installers
from [ShiningLight](https://slproweb.com/products/Win32OpenSSL.html).

### .NET Bindings

By far, the easiest way to use the .NET bindings is just to install the Nuget package we publish:

```sh
PM> Install-Package opendnp3
```

or to allow pre-release versions:

```sh
PM> Install-Package opendnp3 -Pre
```

**Manually building**

Building the .NET bindings requires linking to openssl. As a result, you need to create and install an opendnp3 build
with -DFULL=ON set when creating the SLN.

The .NET bindings use a separate SLN located in the 'dotnet' folder (bindings.sln). They treat the C++ libraries as if they were a dependency. There are a few environment variables you need to define so that the SLN can find opendnp3.

* OPENDNP3_DIR - The directory where opendnp3 was installed, e.g. your argument to CMAKE_INSTALL_PREFIX
* OSSL_LIB32_DIR - The directory where the 32-bit openssl libraries are installed, e.g. C:\OpenSSL-Win32\lib\VC\static
* OSSL_LIB64_DIR - The directory where the 64-bit openssl libraries are installed, e.g. C:\OpenSSL-Win64\lib\VC\static

Using the "static" directory of the openssl distribution will create .NET bindings that statically link all of openssl. If you'd prefer to link dynamically use
the "lib/VC" directory instead.

You don't need to define both the 32/64 bit library locations. Just define what you plan on building.
