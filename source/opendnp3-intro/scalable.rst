
==========================================
Event-driven scalability
==========================================

**Protocol stack anti-patterns**

Many implementations of the protocol use a thread per communication channel when connecting a master to many outstations. 
This type of architecture doesn't scale well. It creates a lot of thrasing and context switching.
Other implementations use a single-thread with non-blocking I/O. This design is efficient, but has parallelization issues. 
If the one thread is ever blocked, other connections are dead in the water.

.. image:: /img/no-blocking.gif

**Event-driven, asynchronous, and multi-core**

Opendnp3 borrows a design pattern from modern web servers: a purely asynchronous programming model driven by a thread pool.
Opendnp3 uses 100% event based I/O taking advantage of your OS's best socket multiplexing abstractions.
This means that your CPU doesn't churn while your program is idle and you don't have to make a trade-off between latency and CPU usage.
Opendnp3 achieves this efficiency by using `ASIO <http://think-async.com/>`_ to handle all timers and I/O requests. 
If you're curious how your specific platform implements the asynchronous I/O have a look `here <http://think-async.com/Asio/asio-1.10.2/doc/asio/using.html>`_.

One way to understand why this is so important is to consider how your computer does work concurrently. 
You can only truly do as many things concurrently as you have threads or cores. 
Your operating system must then fake concurrency for the extra threads by multi-tasking using interrupts to force switches between them. 
Many active threads leads to excessive `context switching <http://en.wikipedia.org/wiki/Context_switch>`_ and memory usage. 
A more efficient methodology is to only use the number of threads that your system can actually run concurrently, aka a thread pool sized to the native concurrency of your system. 
The magic trick is then to NEVER BLOCK any of them so that they're all always doing useful work as fast as they can. 
This leads to efficient CPU utilization, lower memory usage, and lower average latency for servicing requests under load.
Opendnp3 uses 100% non-blocking I/O under the hood. 
That's why it scales so well. Many systems in the industry have a hard time bringing up and maintaining more than a couple hundred socket connections. 
Opendnp3 easily scales right up to the operating system limit, all while using a small amount of CPU and RAM.

