
All opendnp3 programs begin by creating a `DNP3Manager`.  Creating this object allocates a thread pool used
to process events and callbacks to user code.

```c++
// Create a root DNP3 manager with a single thread that logs to the console
DNP3Manager manager(1, ConsoleLogger::Create());
```

If you're familiar with using ASIO in other contexts, than it should be no surprise that the DNP3Manager owns an `asio::io_context`.
It also has a thread pool for calling `asio::io_context::run()`, however, this is an internal detail that most opendnp3 programmers do
not need to know to use the stack.

How many threads you allocate to your thread pool can be a subtle matter. On simple systems like a small outstation that only
talks to a single master, one thread is sufficient. For masters that may talk to hundreds or thousands of outstations, you'll
want to scale your thread-pool to the number of logical processors on your machine.

```c++
// Create a root DNP3 manager as many threads as logical processors
DNP3Manager manager(std::hardware_concurrency(), ConsoleLogger::Create());
```

!!! warning
    You should **avoid blocking** the stack during callbacks made to user code, as there are a limited
	number of threads in the thread pool.
	
This advice is especially critical for large systems where the number of communication channels greatly outnumbers the number of threads
in the pool. If all of your threads are blocked then other channels can't do useful work like sending control requests to the field. Blocking
operations should be done in separate threads. If you must design your system to do some blocking operations without handling other threads,
you can mitigate this problem by scaling the number of threads in the pool as a multiple of the number of cores.

```c++
// Create a root DNP3 manager with twice as many threads as logical processors
DNP3Manager manager(2*std::hardware_concurrency(), ConsoleLogger::Create());
```

Properly configuring your thread pool ensures optimal performance.

### Channels & Sessions

Communication channels are created from the root DNP3Manager class.

```c++
// Create a TCP client channel to which we can bind masters or outstations
auto channel = manager.AddTCPClient(...arguments...);
```

There is a unique method for adding each supported channel type, `TCPClient`, `TCPServer`, `TLSClient`, `TLSServer`, or `Serial`. You should refer to code documentation
for a description of the parameters.

With your channels created you can now bind master or outstations sessions to your them.  Binding multiple master or outstations sessions to a single channel
is a _multi-drop_ configuration.

```c++
// Create a master bound to a particular channel
auto master = channel->AddMaster(...arguments...);

// Create an outstation bound to particular channel
auto outstation = channel->AddOutstation(...arguments...);
```

### Architecture

Each channel and the sessions bound to it, are a single-threaded state-machine.  During execution, ASIO guarantees that each channel
is only processing one event at a time from a single thread. This means that there is no explicit thread synchronization required anywhere in the stack.
When user code wants to communicate with a stack, e.g. load measurement data into an outstation or request that a command be initiated
on a master, it gets "posted" to the correct channel's executor. This ensures that each channel and all the sessions bound to it are
only ever touched by a single thread at a time.

User code, however, may need to worry about multi-threading. If you hand the same callback interface to multiple sessions, you will
potentially receive callbacks from multiple threads simultaneously on the same interface.

![threading](../img/threading.svg)
