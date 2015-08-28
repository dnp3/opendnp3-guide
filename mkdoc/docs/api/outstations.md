### Creating an outstation

An outstation in opendnp3 is a component that communicates with a single master via a communication channel. It makes measurements of the physical world and then 
sends them to a master upon request (solicited) or on its own accord (unsolicited). Occasionally a master requests that it do something by sending it a control. 
Just like a master, an outstation can be attached to any communication channel that opendnp3 supports.

To add an outstation to a communication channel you call the *AddOutstation* method on the channel interface:

```c++
SlaveStackConfig stackConfig;
	
// You must specify the shape of your database and the size of the event buffers
stackConfig.dbTemplate = DatabaseTemplate::AllTypes(10);
stackConfig.outstation.eventBufferConfig = EventBufferConfig::AllTypes(10);
	
// you can override an default outstation parameters here
stackConfig.outstation.params.allowUnsolicited = true;

// You can override the default link layer settings here
// in this example we've changed the default link layer addressing
stackConfig.link.LocalAddr = 10;
stackConfig.link.RemoteAddr = 1;

IOutstation* outstation = channel->AddOutstation(
    "outstation",								// alias for logging
    SuccessCommandHandler::Instance(),			// ICommandHandler (interface)
	DefaultOutstationApplication::Instance(),	// IOutstationApplication (interface)
    stackConfig									// static stack configuration
);

outstation->Enable();

```