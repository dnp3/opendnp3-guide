Creating a master
=================

A master in Opendnp3 is a component that communicates with a single outstation via a communication channel. You may see this term used in other places to refer to a collection of such components communicating with multiple outstations. When more than one master is bound to a single communication channel, it is called a multidrop configuration. This refers to the way in which an RS-485 serial network is chained from device to device. Opendnp3 will let you add multiple masters / outstations to any communication channel, regardless of he underlying transport. You could even bind a master to a TCP server and reverse the normal connection direction. Opendnp3 doesn't care.

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

**Seqence of Events (SOE)**

The primary purpose of DNP3 is deliver a sequence of events (measurement history) from outstation to master. When creating a master, you must supply an instance of the *ISOEHandler* interface. The *PrintingSOEHandler* singleton provided above simply prints all measurements received to the console. The instance provided by your application will do something more interesting like loading to a database, handing off to a protcol translation routine, or simply logging to file.

.. code-block:: cpp

  class ISOEHandler : public ITransactable
  {
    public:
    
    virtual void LoadStatic(const IterableBuffer<IndexedValue<Binary, uint16_t>>& meas) = 0;
    virtual void LoadStatic(const IterableBuffer<IndexedValue<DoubleBitBinary, uint16_t>>& meas) = 0;
    // more types follow ...
    
    virtual void LoadEvent(const IterableBuffer<IndexedValue<Binary, uint16_t>>& meas) = 0;
    virtual void LoadEvent(const IterableBuffer<IndexedValue<DoubleBitBinary, uint16_t>>& meas) = 0;
    // more types follow ...
  }

The master will call ITransactable::Start() prior to any calls to LoadX(X) and will call ITransactable::End() last. The master does this for each application layer fragment containing measurement data received from the outstation, allowing you to individually handle them. If the master does an integrity poll and receives a 4 fragment response, the SOE handler will be called 4 times with a complete Start()/Load(X)/End() cycle.

It is important to remember that while the master is calling you back with measurement updates, it can't do any other work like react to user requests (e.g. controls). It's OK to do a little work like translating the data types, etc during the callbacks, but it's best to offload any blocking I/O like writing to a database to another thread.
