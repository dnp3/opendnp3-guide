The DNP3 link-layer is the lowest level of the DNP3 stack, and provides a number of services for DNP3 communications:

* Addressing - Each DNP3 frame contains both a 16-bit `source` and `destination` address field.
* Keep-alive - The ability to periodically send "keep-alive" requests (`REQUEST_LINK_STATUS`)
* Error-checking - Interleaved CRC values that can separately detect data corruption in the header and payload.

### Configuration

Both master and outstation sessions require link-layer configuration.

The `MasterStackConfig` and `OutstationStackConfig` configuration structures each contain a `LinkConfig` structure. 
This gives you access to the link-layer parameters when creating a master or outstation session.

```c++
MasterStackConfig stackConfig;		// could also be OutstationStackConfig for an outstation

// configure master specific parameters
......	

// set link-layer parameters
stackConfig.link.LocalAddr = 1;		// the address of the master
stackConfig.link.RemoteAddr = 10;   // the address of the remote outstation
```

!!! important
    Not having the link-layer Local/Remote addresses configured correctly is the most frequent source of communication problems.
	Opendnp3 example applications automatically talk to each other using a master address of 1 and an outstation address of 1024.
	There is no standard default address.

### Keep-alives

The link-layer will send a keep-alive request whenever it hasn't received a message from the other side of the link within the `LinkConfig.KeepAliveTimeout` parameter. 
This configurable parameter defaults to 1 minute. It is generally only needed for quiescent TCP operations, and can be disabled for other types of configurations. It is an 
indispensable parameter for polled outstations that act as TCP servers. Writing to a socket is the only way to detect dead/half-open sockets.

### ILinkListener

The `IMasterApplication` and `IOutstationApplication` interfaces inherit from `ILinkListener`. You can monitor important events at the link-layer by overriding the 
default methods on this interface.

```c++
class ILinkListener
{
public:

	/// Called when a the reset/unreset status of the link layer changes
	virtual void OnStateChange(LinkStatus value) {}

	/// Called when the keep alive timer elapses. This doesn't denote a keep-alive failure, it's just a notification
	virtual void OnKeepAliveInitiated() {}

	/// Called when a keep alive message (request link status) receives no response
	virtual void OnKeepAliveFailure() {}

	/// Called when a keep alive message receives a valid response
	virtual void OnKeepAliveSuccess() {}
};
```

The DNP3 specification details specific actions the master or outstation should take (like closing a TCP session and reconnecting) when keep-alive failures occur. 
Opendnp3 does not automatically perform these actions. User-code is expected to monitor this callback interface and take appropriate actions based on the type of communication
channel in use.
