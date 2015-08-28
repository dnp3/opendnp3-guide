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

### MeasUpdate

When a new measurement is read from an input or a new value is received from a downstream protocol, you need to update the corresponding
value in the outstaion. This is accomplished with the _MeasUpdate_ helper class.


```c++
// start a measurement update transaction
MeasUpdate tx(outstation);
tx.Update(Counter(123), 0);		// change a counter value at index 0
tx.Update(Analog(-7.4), 8);		// change an analog value at index 8
```

The update is applied to the outstation when the _MeasUpdate_ instance destructs. This means that the update is atomic.
All of the updated values are applied to the outstation database and event buffers at the same time.

The outstation automatically decides if these update produce _events_. If the value or measurment _flags_ haven't changed, an event is never produced.
How events are detected are defined within the DNP3 standard, and varies from type to type. Analogs and counters can use _deadbands_ to ensure that
unimportant changes are not reported. Opendnp3 supports deadbanding via database configuration covered in a section below.

### ICommandHandler

When the outstation receives a control request...



