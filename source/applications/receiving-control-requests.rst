==========================
Handling control requests
==========================

**TODO - This section needs a rewrite for 2.0.x**

By now you've probably noticed that I haven't described what the *SuccessCommandHandler::Inst()* call is all about. This parameter is the call back interface that the application must provide to handle control requests from a master. The singleton above simply silently acknowledges all requests. The instance provided by your application will do something more interesting by implementing the *ICommandHandler* interface. If you are unfamiliar with control types and modes of operation you should read :ref:`control-types-and-modes`

.. code-block:: java

   // Java
   public interface CommandHandler {
      
       CommandStatus select(ControlRelayOutputBlock command, long index);
       CommandStatus select(AnalogOutputInt32 command, long index);

       // .. more signatures follow
   }


The interface has a method signature for each mode of operation and control type pair. This allows you to individually decide how and what to support in your outstation. It may seem like a lot of work, but just remember that the 4 AnalogOutput types all refer to the same output, just with a different variation from the master.

**How to handle SELECT**

You may have noticed the select signatures.  The outstation stack needs to know what indices you will handle if a valid Select-before-Operate (SBO) sequence occurs when it receives the SELECT request. You shouldn't perform the control in this callback, just do all the validation you would normally do before. If you would handle the control on that index if the function code was an Operate, then return SUCCESS. In rare cases, some outstations might prepare to do a control, i.e. if something needs to be warmed up just in case a valid operate follows the select.
