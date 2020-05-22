On Windows, CMake is used to generate a Visual Studio Solution (SLN) and
associated project files.

```sh
> mkdir build; cd build
> cmake .. <options>
```

Once you've generated your SLN, you can just open it and use Visual Studio to
build the project for you just like any other project.

### Command Line

Alternatively, you can just invoke msbuild.exe on the generated solution and
build from the command line.

```sh
> msbuild opendnp3.sln
or
> cmake --build .
```

### Installing

CMake creates a special project called "install" that you can run inside Visual
Studio to install the headers and libraries to the directory specified by
`CMAKE_INSTALL_PREFIX`.

### TLS Support

If you need to build the stack w/ TLS support (or you're using the .NET
bindings), then you need to install OpenSSL on Windows. Use the full development
package installers from
[ShiningLight](https://slproweb.com/products/Win32OpenSSL.html). CMake is able
to find the OpenSSL libraries and headers in the default install locations used
by the installer.

### .NET Bindings

By far, the easiest way to use the .NET bindings is just to install the [Nuget
package](https://www.nuget.org/packages/opendnp3) we publish:

```sh
PM> Install-Package opendnp3
```

or to allow pre-release versions:

```sh
PM> Install-Package opendnp3 -Pre
```

You will need to have OpenSSL 1.1.1 installed on your system and available in
your path environment.

If you have an error stating "Could not load file or assembly
'DNP3CLRAdapter.dll' or one of its dependencies. The specified module could not
be found.", it means that OpenSSL is not installed. Make sure that
`libcrypto.dll` and `libssl.dll` are installed and available in your path
environment.

#### Building

If you need to build them from scratch, you can enable it when configuring
CMake, e.g.:

```
> cmake .. -DDNP3_DOTNET=ON
```


