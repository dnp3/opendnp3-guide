### Visual Studio SLN

On windows, CMake is used to generate a SLN and associated project files.

```sh
> cmake ../dnp3 <options>
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

