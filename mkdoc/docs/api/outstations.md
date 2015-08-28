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

When the outstation receives a control request, it dispatches individual commands in the message to the ICommandhandler interface
supplied when the outstation was added to the channel.

```
class ICommandHandler : public ITransactable
{
	virtual CommandStatus Select(const ControlRelayOutputBlock& command, uint16_t index) = 0;
	virtual CommandStatus Operate(const ControlRelayOutputBlock& command, uint16_t index) = 0;
	/// ... additional methods for the 4 types of analog outputs - Group 41Var[1-4]
}
```

You'll notice that the interface is _transactable_ meaning that it has Start()/End() methods just like the _ISOEHandler_ interface in the master. ASDUs
can contain multiple controls in a single object header, and possibly multiple headers. The Start()/End() methods tell you when an ASDU containing
commands begins and ends. Many applications probably don't care, but this knowledge is there if you need it for some reason.

The _Select_ operation shouldn't actually perform the command. Think of it as a question along the lines of _"Is this operation supported?"_. 
_Select-Before-Operate_ (SBO) is an artifact of the days before the SCADA community really trusted CRCs. It's  a 2-pass control scheme where the 
outstation verifies that the select/operate are identical. It was intended as an additional protection against data corruption on noisy networks.

The _Operate_ method could really be from a successul SBO sequence or from a _DirectOperate_ or _DirectOperateNoAck_ request. You don't really need to know because the
DNP3 spec says you have to support all modes.

### CommandStatus

You must immediately return a _CommandStatus_ enumeration value in response to each callback. This callback should never block.

```c++
enum class CommandStatus : uint8_t
{
  /// command was accepted, initiated, or queued
  SUCCESS = 0,
  /// command timed out before completing
  TIMEOUT = 1,
  /// command requires being selected before operate, configuration issue
  NO_SELECT = 2,
  /// more values ...
}
```

The enumeration contains about ~18 different values, and you should refer to 1815 or the code comments for a description of each. In general,
you'll be choosing _SUCCESS_ or some kind of error code.

It's important to understand that _SUCCESS_ doesn't imply that the command was synchronously executed. It really just means that the command
was received and queued. Some devices can synchronously process a command, e.g. quickly writing to memory mapped I/O, but you'd never 
want to block in a gateway application to perform a downstream Modbus transaction. You'd pass the control of to another thread or queue the operation
in some way for subsequent processing.