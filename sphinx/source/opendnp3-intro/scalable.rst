
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

Many SCADA masters have a hard time bringing up and maintaining more than a couple hundred socket connections. 
Opendnp3 easily scales right up to the operating system limit, all while using a small amount of CPU and RAM.

