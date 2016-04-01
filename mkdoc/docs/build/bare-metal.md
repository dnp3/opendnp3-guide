### Bare metal systems

The _openpal_ library and outstation components of the _opendnp3_ library have following properties that make them suitable
for small embedded systems.

* Only uses lightweight C++11 frontend abstractions like lambdas.
* No dependency on the C++ standard library or usage of the STL. Typically only new/delete must be defined in terms of malloc/free.
* No runtime heap allocation. All dynamic allocation is performed during initialization to create various buffers.
* Logging uses snprintf instead of string streams and can optionally be stripped out entirely using the preprocessor.

### Porting

The first step in creating a port to an embedded system is creating an implementation of _openpal::IExecutor_ and associated
classes that is appropriate for your target.

Past releases have included [AVR](https://github.com/automatak/dnp3/tree/2.0.1/embedded/atmelavr) and
[ARM](https://github.com/automatak/dnp3/tree/2.0.1/embedded/atmeldue) examples using the Atmel toolchain. These
might be revived at some point in an external repository.
