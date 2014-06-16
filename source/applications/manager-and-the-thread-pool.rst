DNP3 Manager and the Thread Pool
================================

Programs, regardless of how complex, need an instance of the manager class. It's the starting point for adding communication channels, tapping into the logging sub-system, etc. The manager class is given a number when it is constructed. This parameter tells the stack how many threads to allocate to its thread pool. Even though the underlying stack is completely asynchronous and uses all non-blocking calls, the thread pool allows the stack to scale up and stay responsive. Even when a callback to users code is slow, the other connections keep on doing work. Here are the simplest possible programs that instantiate a manager instance. The rest of the tutorials will assume you have this basic infrastructure.

.. code-block:: cpp
   
   // C++11
   #include <opendnp3/DNP3Manager.h>
   #include <thread>

   using namespace opendnp3;

   int main(int argc, char* argv[])
   {
     // hardware_concurrency() gives hint of number of hardware thread contexts
     DNP3Manager manager(std::thread::hardware_concurrency());
   }


.. code-block:: java

   // Java
   import com.automatak.dnp3.DNP3Manager;
   import com.automatak.dnp3.impl.DNP3ManagerFactory;

   public class Main 
   {

       public static void main(String[] args)
       {
          DNP3Manager manager = DNP3ManagerFactory.createManager(
            Runtime.getRuntime().availableProcessors()
          );
          manager.shutdown();
       }
   }


.. code-block:: csharp

   // C#
   using DNP3.Adapter;
   using DNP3.Interface;

   class Main 
   {
     static void Main(string[] args)
     {
       var manager = DNP3ManagerFactory.CreateManager(
         Environment.ProcessorCount
       );
     }
   }

