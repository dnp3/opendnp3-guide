================================
Platform Abtraction Layer (PAL)
================================

Opendnp3 uses a platform abstraction layer (PAL) consisting of a few related interfaces to maintain portability. For common operating systems, the *asiopal* library implements the interfaces in the PAL in a generic way using the ASIO library for cross-platforms sockets, serial ports, timers, etc.

If your OS isn't supported by ASIO or you want to run opendnp3 on a bare-metal system, you'll have to implement the PAL yourself.

**How opendnp3 uses the PAL**


  
    
 
  
