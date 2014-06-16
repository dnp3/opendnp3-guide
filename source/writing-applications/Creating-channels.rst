Creating Channels
=================

Internally, communication in opendnp3 happens via an abstract communication channel interface. Opendnp3 supports TCP Client/Server and serial communications currently, but UDP and TLS encrypted TCP will be added in the near future.

The manager that you created previously is now ready to have some channels bound to it. Just adding a channel to the manager does nothing other than create the record. If it's TCP socket or serial port it won't try to open until you bind an outstation or master. A TCP server won't listen for connections yet either. Here's an example of how you go about adding a TCP client. Assume we have a DNP3Manager called 'manager':

.. code-block:: cpp

   //C++11
   IChannel* pClient = manager.AddTCPClient(
     "tcpclient", //name used in log messages
     LEV_INFO,    //lowest log level of messages
     5000,        //connection retry interval mS
     "127.0.0.1", //ip address of remote outstation
     20000);      //port remote outstation is listening on


.. code-block:: java

   //java
   Channel client = manager.addTCPClient(
     "tcpclient",
     LogLevel.INFO,
     5000,
     "127.0.0.1",
     20000);


.. code-block:: csharp

   //C#
   IChannel client = mgr.AddTCPClient(
     "tcpclient",
     LogLevel.INFO,
     5000,
     "127.0.0.1",
     20000);

