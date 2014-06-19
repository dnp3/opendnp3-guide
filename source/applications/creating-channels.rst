===================
Creating channels
===================

The opendnp3 library uses an abstract communication channel interface to send bytes "over the wire". Opendnp3 supports TCP client/server and serial communications currently, but UDP and TLS encrypted TCP will be added in the near future.

The manager that you created previously is now ready to have some channels bound to it. Adding a channel to the manager does not open it immediately. If it's TCP socket or serial port it won't try to open until you bind an outstation or master and enable the stack. A TCP server won't listen for connections yet either. Here's an example of how you go about adding a TCP client. Assume we have a DNP3Manager called 'manager':

.. code-block:: cpp

   auto pClient = manager.AddTCPClient(
     "tcpclient",               // name used in log messages
     filters::NORMAL,           // lowest log level of messages
     TimeDuration::Seconds(2),  // minimum delay for connection rety
     TimeDuration::Minutes(1),  // maximum delay for connection rety
     "127.0.0.1",               // host name (DNS resolved) or ip address of remote endpoint
     20000);                    // port remote endpoint is listening on

The methods for adding a server channel or a serial port are very similar. 

**Exponential Backoff**

All channels specify two timing paramters for the minimum and maximum connection retry times. The channels use an exponential backoff strategy to retries. If you don't want exponential backoff, just set the minimum and maximum to the same value.

For instance if you set the minimum to *TimeDuration::Seconds(3)* and the maximum to *TimeDuration::Seconds(40)* a series of failed connections would have the following time gaps between attempts.

3, 6, 12, 24, 40, 40, .....

