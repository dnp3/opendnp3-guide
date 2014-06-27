========================
Outstation API
========================

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


Just the same as when we created a channel, we provide an alias for the logger and a log level. The SlaveStackConfig class provides the outstation with all of its configuration. It tells the outstation how to behave, how to respond to requests, and what features to enable or disable. A description of what each parameter does is provided in the code documentation for the class. The method returns an interface that represents the outstation. It contains sub-interfaces for doing things like loading measurements.

.. toctree::
  
  loading-measurements
  receiving-control-requests

