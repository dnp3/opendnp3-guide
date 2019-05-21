On windows, CMake is used to generate a Visual Studio Solution (SLN) and associated project files.

```sh
> mkdir build; cd build
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
the directory specified by `CMAKE_INSTALL_PREFIX`.

### TLS Support

If you need to build the stack w/ TLS support (or you're using the .NET bindings), then you need to install openssl on Windows.  Use the installers
from [ShiningLight](https://slproweb.com/products/Win32OpenSSL.html).

### .NET Bindings

By far, the easiest way to use the .NET bindings is just to install the [Nuget package](https://www.nuget.org/packages/opendnp3) we publish:

```sh
PM> Install-Package opendnp3
```

or to allow pre-release versions:

```sh
PM> Install-Package opendnp3 -Pre
```
#### Building

Enable the building of the .NET bindings when configuring CMake, e.g.:

```
> cmake .. -DDNP3_DOTNET=ON
```


