Where's the superloop?
=======================

OpenDNP3 uses 100% event based I/O taking advantage of your operating system's interrupt handling abstractions. This means that your CPU doesn't churn while your program is idle and you don't have to make a trade-off between latency and CPU usage. OpenDNP3 achieves this efficiency by using Boost::ASIO to handle all timers and I/O requests. If you're curious how your specific platform implements the asynchronous I/O have a look `here <http://www.boost.org/doc/libs/1_53_0/doc/html/boost_asio/overview/implementation.html>`_.

Some implementations are based on an embedded design pattern known as the `super loop <http://en.wikibooks.org/wiki/Embedded_Systems/Super_Loop_Architecture>`_. This style is a 30 year regression back to when microcomputers were first introduced. Such applications typically have the following structure:

.. code-block:: cpp

   //some pseudo C code
   void main()
   {
     init(); // setup resource to be used
     while(true) // repeat forever
     {
       check_for_data(); // check for arrival of data at sockets / serial ports
       check_timers(); // check to see if any timers have expired
       delay_for_next_loop(); // sleep (trade-off between CPU usage and latency)
     }
   }


It forces the application developer to make a trade-off between response latency and CPU usage by deciding how much to wait between each iteration of the loop. It also means the CPU churns while the system is doing nothing at all so that the loop can poll for events.

Operating systems were designed, in part, to avoid this type of architectural configuration. Modern applications are written using event based I/O to avoid needless CPU churn. Be skeptical of any code that asks you to configure or gives you the option of configuring a "LoopPeriod".
