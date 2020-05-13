An outstation in opendnp3 is a component that communicates with a single master via a communication channel. It makes measurements of the physical world and then
sends them to a master upon request (solicited) or on its own accord (unsolicited). Occasionally a master requests that it do something by sending it a control.
Just like a master, an outstation can be attached to any communication channel that opendnp3 supports.

To add an outstation to a communication channel you call the `AddOutstation` method on the channel interface:

```c++
// default configuration w/ an empty database
OutstationStackConfig config;

// configure database points
config.database = DatabaseConfig(10); // 10 of each type with default settings
// adjust some settings for analog 0 to non-default values
config.database.analog_input[0].clazz = PointClass::Class2;
config.database.analog_input[0].svariation = StaticAnalogVariation::Group30Var5;
config.database.analog_input[0].evariation = EventAnalogVariation::Group32Var7;

// configure the size of the event buffers
config.outstation.eventBufferConfig = EventBufferConfig::AllTypes(10);

// you can override a default outstation parameters here
config.outstation.params.allowUnsolicited = true;

// You can override the default link layer settings here
// in this example we've changed the default link layer addressing
config.link.LocalAddr = 10;
config.link.RemoteAddr = 1;

auto outstation = channel->AddOutstation(
  "outstation",                             // alias for logging
  SuccessCommandHandler::Create(),          // ICommandHandler (interface)
  DefaultOutstationApplication::Create(),   // IOutstationApplication (interface)
  config                                    // static stack configuration
);

outstation->Enable();
```

### UpdateBuilder

When a new measurement is read from an input or a new value is received from a downstream protocol, you need to update the corresponding
value in the outstation. This is accomplished with the `UpdateBuilder` and corresponding `Updates` class.


```c++
UpdateBuilder builder;
builder.Update(Counter(state.count), 0);
builder.Update(Analog(state.value), 0);
// ... update more types and indices

// finalize the set of updates
auto updates = builder.Build();

// apply the updates to one or more outstations
outstation->apply(updates);
```

The update is atomic. All of the updated values are applied to the outstation database and event buffers at the same time. The `Updates` instance
returned from `UpdateBuilder::Build()` can be safely sent to any number of outstation instances. The outstation automatically decides if these updates 
produce events. How events are detected are defined within the DNP3 standard, and varies from type to type. Analogs and counters can use deadbands
to ensure that unimportant changes are not reported.

### ICommandHandler

When the outstation receives a control request, it dispatches individual commands in the message to the `ICommandhandler` interface
supplied when the outstation was added to the channel.

```c++
class ICommandHandler
{
   /**
     * called when a command APDU begins processing
     */
    virtual void Begin() = 0;

    /**
     * called when a command APDU ends processing
     */
    virtual void End() = 0;


	virtual CommandStatus Select(const ControlRelayOutputBlock& command, uint16_t index) = 0;

	virtual CommandStatus Operate(const ControlRelayOutputBlock& command,
                                  uint16_t index,
                                  IUpdateHandler& handler,
                                  OperateType opType) = 0;

	/// ... additional methods for the 4 types of analog outputs - Group 41Var[1-4]
}
```

The Begin()/End() methods tell you when an ASDU containing commands begins and ends. Many applications probably don't care, but this 
knowledge is there if you need it for some reason.

The `Select` method shouldn't actually perform the command. Think of it as a question along the lines of _"Is this operation supported?"_.
Select-Before-Operate (SBO) is an artifact of the days before the the CRC integrity checks were really truted. It's a 2-pass control scheme where
the outstation verifies that the select/operate are identical. It was intended as an additional protection against data corruption on noisy
transports like radio and modems.

The `Operate` method is called from a successful SBO sequence or from a `DirectOperate` or `DirectOperateNoAck` request. Applications shouldn't 
care how the request came in, but the `OperateType` parameter provides an enumeration that can be used to reject certain operations or to forward 
the same mode downstream in gateway applications.

```c++
enum class OperateType : uint8_t
{
  /// The outstation received a valid prior SELECT followed by OPERATE
  SelectBeforeOperate = 0x0,
  /// The outstation received a direct operate request
  DirectOperate = 0x1,
  /// The outstation received a direct operate no ack request
  DirectOperateNoAck = 0x2
};
```

The `IUpdateHandler` provides the ability to update points as a direct result of the control. This is a convience since you would
otherwise have to pass your implementation of `ICommandHandler` an instance of `IOutstation` after you create it. Frequently, implemenations
want to mirror binary and analog output operations to binary output status and analog output status points directly in the handler.

### CommandStatus

You must immediately return a `CommandStatus` enumeration value in response to each callback. **This callback should never block**.

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
you'll be choosing `SUCCESS` or some kind of error code.

It's important to understand that `SUCCESS` doesn't imply that the command was synchronously executed. It really just means that the command
was received and queued. Some devices can synchronously process a command, e.g. quickly writing to memory mapped I/O, but you'd never
want to block in a gateway application to perform a downstream Modbus transaction. You'd pass the control of to another thread or queue the operation
in some way for subsequent processing.
