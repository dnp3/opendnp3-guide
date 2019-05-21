On linux, the easiest way to use CMake is just to let it create a makefile for you. You can then use this makefile in the same
way you normally would:

```sh
> git clone --recursive --branch <tag-name> https://github.com/dnp3/opendnp3.git
> cd opendnp3
> mkdir build; cd build
> cmake .. <options>
> make -j
> sudo make install
```


### Cross-compiling

CMake also supports cross-compiling to embedded-linux platforms. How to do this is outside the scope of this documentation, but is 
explained [here](http://www.cmake.org/Wiki/CMake_Cross_Compiling).

The preferred method is to create a toolchain file for each target platform.