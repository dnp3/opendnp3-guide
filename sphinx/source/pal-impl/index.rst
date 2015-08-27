================================
Platform Abstraction Layer (PAL)
================================

Opendnp3 uses a Platform Abstraction Layer (PAL) to maintain portability. The PAL provides a number of services in the form of interfaces. The openpal library combines these interfaces with a number of utility classes useful for writing protocol stacks including serialization routines, statically allocated container types, and a general purpose logging facility. The openpal library has the following directory structure.

.. code-block:: java

   /openpal            // openpal library source root
     /channel          // defines an abstract communication channel
	 /container        // various reusable statically-sized container types used by opendnp3
	 /executor         // defines an abstract event queuing service and software timer facility
	 /logging          // a general purpose logging facility with pluggable backends
	 /serialization    // provides little and big endian serializations routines configurable (-DOPENPAL_FLIP_ENDIAN)
	 /util             // various utility functions and classes


The most important interfaces are found in openpal/executor:

* openpal::IExecutor - Interface used to queue events to be executed asynchronously. These events can be run ASAP (Post) or can be scheduled (Start) as timers.
* openpal::ITimer - Interface that represents a pending timer expiration. This is what is returned from the IExecutor::Start methods.

For common operating systems, the asiopal library implements the interfaces in the PAL using the ASIO library for cross-platforms sockets, serial ports, timers, etc. If your OS isn't supported by ASIO or you want to run opendnp3 on a bare-metal system, you'll have to implement the PAL yourself.

.. toctree::
  
  asiopal




  
    
 
  
