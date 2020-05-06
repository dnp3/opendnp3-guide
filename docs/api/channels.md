The library uses abstract communication channels to send and receive bytes "over the wire". Opendnp3 supports TCP client/server,
TLS client/server, UDP, and serial communications.

The manager that you created previously is now ready to have some channels bound to it. Adding a channel to the manager
does not make it attempt to open the channel immediately. If it's a TCP socket or serial port it won't try to open until you bind at least one outstation
or master session and enable it. Here's an example of how you go about adding a TCP client. Assume we have a `DNP3Manager` called 'manager':

```c++
auto channel = manager.AddTCPClient(
  "tcpclient",                            // alias used in log messages
  levels::NORMAL,                         // bitfield used to filter what gets logged
  ChannelRetry::Default(),	              // determines how connections will be retried
  "127.0.0.1",                            // host name (DNS resolved) or IP address of remote endpoint
  "0.0.0.0",                              // adapter on which to attempt the connection (any adapter)
  20000,                                  // port remote endpoint is listening on
  nullptr                                 // optional listener interface for monitoring the channel
);
```

The API for creating TCPServer channels or Serial channels is very similar. Just refer to the code documentation.

### Exponential backoff

The `ChannelRetry` configuration specifies two timing parameters for the minimum and maximum connection retry times using an exponential back-off strategy. If you don't want
exponential back-off, just set the minimum and maximum to the same value for a consistent delay. Exponential back-off really only makes sense for initiating TCP connections.

For instance if you set the minimum to `TimeDuration::Seconds(3)` and the maximum to `TimeDuration::Seconds(40)` a series of failed connections would have the following
time gaps between attempts.


`3, 6, 12, 24, 40, 40, .....`

### Monitoring channels

Most of the time your communication channel is open and passing dnp3 traffic back and forth. Sometimes, however, things can go wrong with your network or you have mis-configured your connection. when
creating your channel, you can pass in a `shared_ptr<IChannelListener>` to monitor the state of channel. This interface provides a method returning an enumeration of the states of the channel:

```c++
enum class ChannelState : uint8_t
{
	/// offline and idle
	CLOSED = 0,
	/// trying to open
	OPENING = 1,
	/// open
	OPEN = 2,
	/// stopped and will never do anything again
	SHUTDOWN = 3
};
```

### Cleaning up

Channels and all the sessions bound to them are automatically cleaned up when the DNP3Manager goes out of scope
(or is deleted if allocated dynamically). You can manually remove a channel without having to stop every master or outstation bound
to it by calling `IChannel::Shutdown()`.

```c++
// permanently shutdown the channel
channel->Shutdown();
```

Calls to Shutdown() are idempotent. The resources for the underlying channel will be freed
when you drop the reference to the `shared_ptr<IChannel>`.
