====================
No Microcontrollers
====================

Microcontrollers (MCUs) were not a target during the early development of opendnp3. The library was originally developed for a specific purpose in 2006 to target this `embedded Linux <http://www.embeddedarm.com/products/board-detail.php?product=TS-7250>`_. The portability of the codebase can be primarily attributed to the excellent `ASIO <http://think-async.com/>`_ library. The 1.1.x branch of opendnp3 does not support bare metal deployments. The 2.0.x branch is factored to allow a custom hardware abstraction layer to be written.

Linux and the open-source / hardware movement have definitively changed the landscape in embedded computing. Bare metal MCUs will have a long lifetime in many applications including hard real-time, however, Linux and ARM will continue to capture more and more market share.  Many companies these days are taking a hierarchical approach to embedded systems whereby real-time control is allocated to an MCU / DSP and higher-level functions such as external communications are the responsibility of an ARM with Linux.
