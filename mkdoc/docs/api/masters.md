### Creating a master

A master in opendnp3 is a component that communicates with a single outstation via a communication channel. You may see this term used in other places to refer to a 
collection of such components communicating with multiple outstations. When more than one master is bound to a single communication channel, it is called a 
_multidrop configuration_.  This refers to the way in which an RS-485 serial network is chained from device to device. Opendnp3 will let you add multiple 
masters / outstations to any communication channel, regardless of he underlying transport. You could even bind a master to a TCP server and reverse the 
normal connection direction. Just like the honey badger, opendnp3 doesn't care.

To add a master to a communication channel you call the *AddMaster* method on the channel interface:

```c++
// Contains static configuration for the master, and transport/link layers
MasterStackConfig stackConfig;

// you can optionally override these defaults like setting the application layer response timeout
// or change behaviors on the master
stackConfig.master.responseTimeout = TimeDuration::Seconds(2);
stackConfig.master.disableUnsolOnStartup = true;
	
// ... or you can override the default link layer settings 
stackConfig.link.LocalAddr = 1;
stackConfig.link.RemoteAddr = 10;
   
IMaster* pMaster = pClient->AddMaster(
	"master",											// alias for logging
	PrintingSOEHandler::Inst(),							// ISOEHandler - interface
	asiodnp3::DefaultMasterApplication::Instance(),		// IMasterApplication - interface
	stackConfig											// static stack configuration
);
```

### ISOEHandler

Note the 2nd parameter in the call to _AddMaster(...)_. This is the user-defined interface used to receive measurement data
that the master has received from the outstation. SOE stands for _Sequence of Events_. SOE is a common term in SCADA circles
that is synonymous with "the order in which things happened".

```
class ISOEHandler : public ITransactable
{
public:
	
	virtual void Process(const HeaderInfo& info, const ICollection<Indexed<Binary>>& values) = 0;
	
	// more Process methods for types like Analog, Counter, etc ....
}
```

An ISOEHandler is just an interface with an overloaded _Process_ method for every measurement type in DNP3. It also inherits
_Start()_ and _End()_ methods from ITransactable. This allows you tell when the master begins and ends parsing a received
ASDU that contains measurement data. You'll see this Start/End pattern with other interfaces in opendnp3.

The _PrintingSOEHandler_ in the snippet where we added the master is just a singleton that prints measurement values to the console.
You'll definitely want to write your own implementation so that you can write to file, database, or display on your application in some 
fashion. The PrintingSOEHandler just extracts the measurement values from the ICollection like the following:

```
void Process(const HeaderInfo& info, const ICollection<Indexed<Binary>>& values) 
{
	auto print = [](const Indexed<Binary>& pair) { 
		std::cout << "[" << pair.index << "] : " << pair.value << std::endl;
	};
	values.ForeachItem(print);
}
```

There's also a wealth of information in the _HeaderInfo_ object including:

* The specific group/variation associated with this ASDU header
* The QualifierCode associated with this header
* An enumeration describing the validity of the timestamp for convienence to the programmer.
* The index of the header within the ASDU

