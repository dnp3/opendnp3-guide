### Creating a channel

The opendnp3 library uses an abstract communication channels to send and receive bytes "over the wire". Opendnp3 supports TCP client/server
and serial communications currently, but UDP and TLS encrypted TCP may be added in the future.

The manager that you created previously is now ready to have some channels bound to it. Adding a channel to the manager 
does not make it attempt to open immediately. If it's a TCP socket or serial port it won't try to open until you bind at least one outstation 
or master session and enable it. Here's an example of how you go about adding a TCP client. Assume we have a DNP3Manager called 'manager':

```c++
IChannel* channel = manager.AddTCPClient(
  "tcpclient",                // alias used in log messages
  levels::NORMAL,             // bitfield used to filter what gets logged
  TimeDuration::Seconds(2),   // minimum delay for connection rety
  TimeDuration::Minutes(1),   // maximum delay for connection rety
  "127.0.0.1",                // host name (DNS resolved) or ip address of remote endpoint
  "0.0.0.0",				  // adapter on which to attempt the connection (any adapter)
  20000						  // port remote endpoint is listening on
);
```

The API for creating TCPServer channels or Serial channels is very similar. Just refer to the code documentation.

### Exponential backoff

All channels specify two timing parameters for the minimum and maximum connection retry times. The channels use an exponential backoff strategy for retries. If you don't want
exponential backoff, just set the minimum and maximum to the same value for a consistent delay. Exponential-backoff really only makes sense for initiating TCP connections.

For instance if you set the minimum to *TimeDuration::Seconds(3)* and the maximum to *TimeDuration::Seconds(40)* a series of failed connections would have the following 
time gaps between attempts.

3, 6, 12, 24, 40, 40, .....

### Monitoring channels

Most of the time your communication channel is open and passing dnp3 traffic back and forth. Sometimes, however, things can go wrong with your network or you have mis-configured 
your connection. The communication channel interface offers a way to monitor the state of channel. These states are represented by an enumeration:

```c++
enum class ChannelState : uint8_t
{
	/// offline and idle
	CLOSED = 0,
	/// trying to open
	OPENING = 1,
	/// waiting to open
	WAITING = 2,
	/// open
	OPEN = 3,
	/// stopped and will never do anything again
	SHUTDOWN = 4
};
```

Listeners can be bound using a c++11 lambda expression. All callbacks come from the thread pool, so remember not to block.

```c++
channel->AddStateListener([](ChannelState state){
    cout << "state: " << ConvertChannelStateToString(state) << endl;
});
```

### Cleaning up channels

Channels and all the sessions bound to them are automatically cleaned up when the DNP3Manager goes out of scope 
(or is deleted if allocated dynamically). You can manually remove a channel without having to stop every channel by calling
_IChannel::Shutdown()_.

```c++
// permanently shutdown the channel
channel->Shutdown();
```

This actually ends up deleting the pointer to the channel itself, so be sure to not use the IChannel pointer once you've called shutdown.
It also automatically cleans up all bound sessions, so any pointers to masters/outstations you had that were bound to the channel are
also now freed.
