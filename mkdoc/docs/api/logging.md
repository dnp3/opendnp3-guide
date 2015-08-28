### Listening

You can bind to the opendnp3 log stream by calling _DNP3Manager::AddLogSubscriber_ and passing in an implementation of the _ILogHandler_ interface. This interface
just has a single method:

```c++
class ILogHandler
{
public:	
	virtual void Log( const LogEntry& entry ) = 0;
};
```

You can bind as many log handlers as you like to the DNP3Manager. For instance, you might have one that prints to a console and one that logs to a file or database. Keep
in mind that this is a callback from the thread-pool and will block that thread from executing. A good strategy for a file-logger in a big system would be to send a message 
to a worker thread to write the message to disk.

### Individual loggers

Each channel has its own _Logger_ whose log level can be individually configured and adjusted at run-time. This allows you increase the log-level of a particular channel, without 
receiving full protocol analysis for every channel and grinding the system to a halt. You specify the initial log level for each channel when it is created. You can adjust it
during execute by calling _IChannel::SetLogFilters_.

### Log levels

Opendnp3 defines a number of typical log levels like DEBUG, INFO, WARN, ERR. In addition it defines a number of log levels specific to the protocol dissection and analysis. Turning 
on all the various DNP3 analysis levels allows you to create useful communication traces like the following.

```sh
ms(1440709781929) <-TL--  outstation - FIR: 1 FIN: 1 SEQ: 0 LEN: 11
ms(1440709781932) <-AL--  outstation - C0 15 3C 02 06 3C 03 06 3C 04 06
ms(1440709781934) <-AL--  outstation - FIR: 1 FIN: 1 CON: 0 UNS: 0 SEQ: 0 FUNC: DISABLE_UNSOLICITED
ms(1440709781939) <-AL--  outstation - 060,002 - Class Data - Class 1 - all objects
ms(1440709781944) <-AL--  outstation - 060,003 - Class Data - Class 2 - all objects
ms(1440709781949) <-AL--  outstation - 060,004 - Class Data - Class 3 - all objects
```
