========================
Receiving measurements
========================

**Seqence of Events (SOE)**

The primary purpose of DNP3 is deliver a sequence of events (measurement history) from outstation to master. When creating a master, you must supply an instance of the *ISOEHandler* interface. The *PrintingSOEHandler* singleton provided above simply prints all measurements received to the console. The instance provided by your application will do something more interesting like loading to a database, handing off to a protocol translation routine, or simply logging to file.

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

