Creating an outstation
======================

An outstation is a component that communicates with one or more masters via a communication channel. It makes measurements of the physical world and then sends them to a master upon request (solicited) or on its own accord (unsolicited). Occasionally a master requests that it do something by sending it a control.  Just like a master, an outstation can be attached to any communication channel that Opendnp3 supports.

To add an outstation to a communication channel you call the *AddOutstation* method on the channel interface:

.. code-block:: cpp

   SlaveStackConfig stackConfig;

   IOutstation* pOutstation = pChannel->AddOutstation(
     "outstation",                  // stack name
     LEV_INFO,                      // log filter level
     SuccessCommandHandler::Inst(), // see below
     stackConfig                    // stack configuration
   );


Just the same as when we created a channel, we provide a string ID for the logger and a log level. The SlaveStackConfig class provides the outstation with all of its configuration. It tells the outstation how to behave, how to respond to requests, and what features to enable or disable. A description of what each parameter does is provided in the code documentation for the class. The method returns an interface that represents the outstation. It contains sub-interfaces for doing things like loading measurements.

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
