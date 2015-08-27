===================
Master API
===================

A master in Opendnp3 is a component that communicates with a single outstation via a communication channel. You may see this term used in other places to refer to a collection of such components communicating with multiple outstations. When more than one master is bound to a single communication channel, it is called a multidrop configuration. This refers to the way in which an RS-485 serial network is chained from device to device. Opendnp3 will let you add multiple masters / outstations to any communication channel, regardless of he underlying transport. You could even bind a master to a TCP server and reverse the normal connection direction. Just like the honey badger, Opendnp3 doesn't care.

To add a master to a communication channel you call the *AddMaster* method on the channel interface:

.. code-block:: cpp

   MasterStackConfig stackConfig;  // initialized with defaults

   // you can optionally override these defaults like setting the application layer response timeout ...
   stackConfig.master.responseTimeout = TimeDuration::Seconds(2);
	
   // ... or you can override the default link layer settings 
   stackConfig.link.LocalAddr = 1;
   stackConfig.link.RemoteAddr = 10;
   
   IMaster* pMaster = pClient->AddMaster(
	                   "master",                        // alias for logging
	                   PrintingSOEHandler::Inst(),      // callback for data processing, this single just prints to measurements to console
	                   asiopal::UTCTimeSource::Inst(),  // UTC time source for time synchronization, this one just pulls from system clock
	                   stackConfig                      // stack configuration
	               );

Just the same as when we created a channel, we provide an alias for the logger and a log level. The *MasterStackConfig* instance provides all of the configuration. It defines the default master behavior. A description of what each parameter does is provided in the code documentation for the class. The method returns an interface that represents the master.  It contains sub-interfaces for doing things like issuing controls.

.. toctree::
  
  receiving-measurements
  making-control-requests