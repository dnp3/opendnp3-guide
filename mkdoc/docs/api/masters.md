### Creating a master

A master in opendnp3 is a component that communicates with a single outstation via a communication channel. You may see this term used in other places to refer to a 
collection of such components communicating with multiple outstations. When more than one master is bound to a single communication channel, it is called a 
_multidrop configuration_.  This refers to the way in which an RS-485 serial network is chained from device to device. Opendnp3 will let you add multiple 
masters / outstations to any communication channel, regardless of he underlying transport. You could even bind a master to a TCP server and reverse the 
normal connection direction.

To add a master to a communication channel you call the *AddMaster(...)* method on the channel interface:

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
   
IMaster* master = channel->AddMaster(
	"master",											// alias for logging
	PrintingSOEHandler::Instance(),						// ISOEHandler (interface)
	asiodnp3::DefaultMasterApplication::Instance(),		// IMasterApplication (interface)
	stackConfig											// static stack configuration
);

// enable the master - you can also Disable() it or Shutdown() permanently
master->Enable();

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

**Remember that the callbacks for the ISOEHandler methods come from the thread-pool.** Depending on the number of sessions, you may not
want to block the stack in these callbacks. You might consider allocating some kind of object that is passed to a worker thread
to actually write the data to disk/database.


### IMasterApplication

The 3rd parameter in the call to _AddMaster(...)_ is a user-defined interface called _IMasterApplication_. It contains
inherits from two sub-interfaces _ILinkListener_ and _IUTCTimeSource_ as well as adding a number of methods that are
master specific.

You can see all the methods you can override in the code documentation, but the most important ones are:

* _void IOnReceiveIIN(const IINField& iin)_ - Notifies you whenever an ASDU is received containing an internal indication field
 (IIN field). This allow you to log and react to specific error bits returned by the device.

* _void OnTaskComplete(const TaskInfo& info)_ - Tell you about tasks that are built into the master succeeding/failing. This callback
is usually used to assess the "health" of the session.

The ILinkListener interface is also used on IOutstationApplication and is described in its own section.

### MasterStackConfig

The final parameter passed into _AddMaster(...)_ is configuration struct that consists of link-layer configuration
information and static configuration that defines that masters behavior. The link-layer config is also used for outstations,
and is described in its own section.

### MasterParams

Each of the dozen or so fields in this struct control certain automated behaviors within the master. Refer to the code documentation
for complete descriptions. This struct controls behaviors like:

* The default response timeout
* Whether to perform unsolcited disable/enable on startup
* Whether to perform automatic time synchronization if requested
* The maximum Tx/Rx ASDU size which always default to 2K as per the DNP3 specification

### IMaster

When you added a master to the channel, the channel returned an _IMaster_ interface. This interface provides all access to a number of operations
on the master. Refer to the code documentation for specifics. Some examples are:

* Add periodic scans to the master like exception (Class 1/2/3) and integrity scans (Class 1/2/3/0)
* Scanning for specific ranges or event counts
* The _ICommandProcessor_ sub-interface allow you to do _SelectBeforeOperate_ and _DirectOperate_ requests w/ CROBs and Analog Outputs

### Cleaning Up

Masters are automatically destroyed when their channel is shutdown or when the underlying DNP3Manager is destroyed. If you want to clean them up 
manually w/o deallocated the underlying channel, you can call Shutdown(). When you do this the IMaster pointer is deleted so don't use it after being
shutdown!