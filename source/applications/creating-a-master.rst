Creating a master
=================

A master in Opendnp3 is a component that communicates with a single outstation via a communication channel. You may see this term used in other places to refer to a collection of such components communicating with multiple outstations. When more than one master is bound to a single communication channel, it is called a multidrop configuration. This refers to the way in which an RS-485 serial network is chained from device to device. Opendnp3 will let you add multiple masters / outstations to any communication channel, regardless of he underlying transport. You could even bind a master to a TCP server and reverse the normal connection direction. Opendnp3 doesn't care.

To add a master to a communication channel you call the *AddMaster* method on the channel interface:

.. code-block:: cpp

   // C++
   MasterStackConfig stackConfig;  // initialized with defaults

   IMaster* pMaster = pChannel->AddMaster(
     "master",                     // stack name
     LEV_INFO,                     // log filter level
     PrintingDataObserver::Inst(), // see below
     stackConfig                   // stack configuration
   );


Just the same as when we created a channel, we provide a string ID for the logger and a log level. The *MasterStackConfig* class provides the master with all of its configuration. It tells the master how to behave, what scans to do, and what features to enable or disable on the outstation. A description of what each parameter does is provided in the code documentation for the class. The method returns an interface that represents the master. It contains sub-interfaces for doing things like issuing controls.

By now you've probably noticed that I haven't described what the *PrintingDataObserver::Inst()* call is all about. This parameter is the call back interface that the application must provide to receive measurements as they are received from the outstation. The singleton above simply prints all measurements received to the console. The instance provided by your application will do something more interesting by implementing the *IDataObserver* interface.

.. code-block:: java

   // Java
   public interface DataObserver {    
       void start();
       void update(BinaryInput meas, long index);
       void update(AnalogInput meas, long index);
       void update(Counter meas, long index);
       void update(BinaryOutputStatus meas, long index);
       void update(AnalogOutputStatus meas, long index);
       void end();
   }


The master will call start() prior to any calls to update(X) and will call end() last. The master does this for each application layer fragment containing measurement data received from the outstation, allowing you to individually handle them. If the master does an integrity poll and receives a 4 fragment response, the data observer will be called 4 times with a complete start()/update(X)/end() cycle.

It is important to remember that while the master is calling you back with measurement updates, it can't do any other work like react to control requests. It's OK to do a little work like translating the data types, etc during the callbacks, but it's best to offload any blocking I/O like writing to a database to another thread.
