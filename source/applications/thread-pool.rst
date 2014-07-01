
================================
Manager and the thread pool
================================

Opendnp3 applications on an operating system start with an instance of the *asiodnp3::DNP3Manager* class. It's the starting point for adding communication channels, tapping into the logging sub-system, etc. The manager class is given a concurrency hint when it is constructed. This tells the stack how many threads to allocate to its thread pool. Even though the underlying stack is completely asynchronous and uses all non-blocking calls, the thread pool allows the stack to scale up and stay responsive when there are many concurrent sessions. Even when a callback to users code is slow, the other connections keep doing work. Here is the simplest possible program that instantiates a managers. The rest of the tutorials will assume you have this basic infrastructure.

.. code-block:: cpp
      
   #include <asiodnp3/DNP3Manager.h>
   #include <thread>   

   int main(int argc, char* argv[])
   {
     // hardware_concurrency() gives hint of number of hardware thread contexts
     asiodnp3::DNP3Manager manager(std::thread::hardware_concurrency());

     // create communication channels
     .....

     // add masters / outstations to the communication channels
     .....
   }
