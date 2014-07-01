=======================
Communication channels
=======================

.. toctree::

  monitoring-channels

The opendnp3 library uses an abstract communication channels to send and receive bytes "over the wire". Opendnp3 supports TCP client/server and serial communications currently, but UDP and TLS encrypted TCP may be added in the future.

The manager that you created previously is now ready to have some channels bound to it. Adding a channel to the manager does not make it attempt to open immediately. If it's a TCP socket or serial port it won't try to open until you bind at least one outstation or master session and enable it. Here's an example of how you go about adding a TCP client. Assume we have a DNP3Manager called 'manager':

.. code-block:: cpp

   auto pClient = manager.AddTCPClient(
     "tcpclient",                // alias used in log messages
     levels::NORMAL,             // bitfield used to filter log message
     TimeDuration::Seconds(2),   // minimum delay for connection rety
     TimeDuration::Minutes(1),   // maximum delay for connection rety
     "127.0.0.1",                // host name (DNS resolved) or ip address of remote endpoint
     20000);                     // port remote endpoint is listening on  

The methods for adding a server channel or a serial port are very similar. Refer to the doxygen documentation for details.

**Exponential Backoff**

All channels specify two timing parameters for the minimum and maximum connection retry times. The channels use an exponential backoff strategy to retries. If you don't want exponential backoff, just set the minimum and maximum to the same value.

For instance if you set the minimum to *TimeDuration::Seconds(3)* and the maximum to *TimeDuration::Seconds(40)* a series of failed connections would have the following time gaps between attempts.

3, 6, 12, 24, 40, 40, .....

