Making control requests
=======================

Application code can **_issue_** a control request using the **_CommandProcessor_** interface provided by the Master interface. You can learn about the different control types and operation modes [[here | Control types and modes]].

.. code-block:: java

   // Java
   public interface CommandProcessor {   

       ListenableFuture<CommandStatus> selectAndOperate(ControlRelayOutputBlock command, long index);    
       ....

   }


This interface has a method signature for every control and operating mode pair. In C++ you provide a callback lambda for the result, and in C# and Java a **_future_** of type CommandStatus is returned. A future is an interface that represents the eventual value of the operation. You may either synchronously block for the result of the operation or asynchronously listen. The C# example below issues a CROB using the SBO operation mode and then calls Await() on the future to block the current thread until the control requests is complete:

.. code-block:: csharp

   // C#
   var crob = new ControlRelayOutputBlock(ControlCode.PULSE, 1, 100, 100), index);
   var future = master.GetCommandProcessor().SelectAndOperate(crob);
   CommandStatus result = future.Await();                
   Console.WriteLine("Result: " + result);


Check out the master demo for your language to see the idiomatic way to issue a control request.
