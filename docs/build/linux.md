On linux, the easiest way to use CMake is just to let it create a makefile for you. You can then use this makefile in the same
way you normally would:

```sh
> mkdir build; cd build
> cmake .. <options>
> make -j
> sudo make install
```


### Cross-compiling

CMake also supports cross-compiling to embedded-linux platforms. How to do this is outside the scope of this documentation, but is 
explained [here](http://www.cmake.org/Wiki/CMake_Cross_Compiling).

The preferred method is to create a toolchain file for each target platform.

An example integration on a TI MSP432 is available [here](https://github.com/emgre/opendnp3msp432), although it's quite old.