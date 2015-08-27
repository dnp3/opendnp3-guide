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
receiving full protocol analysis for every channel and grinding the system to a halt. You specifiy the initial log level for each channel when it is created. You can adjust it
during execute by calling _IChannel::SetLogFilters_.

### Log levels

Opendnp3 defines a number of typical log levels like DEBUG, INFO, WARN, ERR. In addition it defines a number of log levels specific to the protocol dissection and analysis. Turning 
on all the various DNP3 analysis levels allows you to create communication traces like the following.

```sh
s(1440709781909) --AL->  outstation - F0 82 80 00
ms(1440709781911) --AL->  outstation - FIR: 1 FIN: 1 CON: 1 UNS: 1 SEQ: 0 FUNC: UNSOLICITED_RESPONSE IIN: [0x80, 0x00]
ms(1440709781917) --TL->  outstation - FIR: 1 FIN: 1 SEQ: 0 LEN: 4
ms(1440709781920) --LL->  outstation - Function: PRI_UNCONFIRMED_USER_DATA Dest: 1 Source: 10 Length: 5
ms(1440709781924) <-LL--  server     - Function: PRI_UNCONFIRMED_USER_DATA Dest: 10 Source: 1 Length: 17
ms(1440709781929) <-TL--  outstation - FIR: 1 FIN: 1 SEQ: 0 LEN: 11
ms(1440709781932) <-AL--  outstation - C0 15 3C 02 06 3C 03 06 3C 04 06
ms(1440709781934) <-AL--  outstation - FIR: 1 FIN: 1 CON: 0 UNS: 0 SEQ: 0 FUNC: DISABLE_UNSOLICITED
ms(1440709781939) <-AL--  outstation - 060,002 - Class Data - Class 1 - all objects
ms(1440709781944) <-AL--  outstation - 060,003 - Class Data - Class 2 - all objects
ms(1440709781949) <-AL--  outstation - 060,004 - Class Data - Class 3 - all objects
ms(1440709781952) --AL->  outstation - C0 81 80 00
ms(1440709781954) --AL->  outstation - FIR: 1 FIN: 1 CON: 0 UNS: 0 SEQ: 0 FUNC: RESPONSE IIN: [0x80, 0x00]
ms(1440709781958) --TL->  outstation - FIR: 1 FIN: 1 SEQ: 1 LEN: 4
ms(1440709781960) --LL->  outstation - Function: PRI_UNCONFIRMED_USER_DATA Dest: 1 Source: 10 Length: 5
ms(1440709781965) <-LL--  server     - Function: PRI_UNCONFIRMED_USER_DATA Dest: 10 Source: 1 Length: 8
ms(1440709781969) <-TL--  outstation - FIR: 1 FIN: 1 SEQ: 1 LEN: 2
ms(1440709781973) <-AL--  outstation - D0 00
ms(1440709781975) <-AL--  outstation - FIR: 1 FIN: 1 CON: 0 UNS: 1 SEQ: 0 FUNC: CONFIRM
ms(1440709781981) <-LL--  server     - Function: PRI_UNCONFIRMED_USER_DATA Dest: 10 Source: 1 Length: 14
ms(1440709781986) <-TL--  outstation - FIR: 1 FIN: 1 SEQ: 2 LEN: 8
ms(1440709781989) <-AL--  outstation - C1 02 50 01 00 07 07 00
ms(1440709781992) <-AL--  outstation - FIR: 1 FIN: 1 CON: 0 UNS: 0 SEQ: 1 FUNC: WRITE
ms(1440709781995) <-AL--  outstation - 080,001 Internal Indications - Packed Format, 8-bit start stop [7, 7]
ms(1440709782000) --AL->  outstation - C1 81 00 00
ms(1440709782002) --AL->  outstation - FIR: 1 FIN: 1 CON: 0 UNS: 0 SEQ: 1 FUNC: RESPONSE IIN: [0x00, 0x00]
ms(1440709782007) --TL->  outstation - FIR: 1 FIN: 1 SEQ: 2 LEN: 4
ms(1440709782009) --LL->  outstation - Function: PRI_UNCONFIRMED_USER_DATA Dest: 1 Source: 10 Length: 5
ms(1440709782016) <-LL--  server     - Function: PRI_UNCONFIRMED_USER_DATA Dest: 10 Source: 1 Length: 20
ms(1440709782020) <-TL--  outstation - FIR: 1 FIN: 1 SEQ: 3 LEN: 14
ms(1440709782022) <-AL--  outstation - C2 01 3C 02 06 3C 03 06 3C 04 06 3C 01 06
ms(1440709782025) <-AL--  outstation - FIR: 1 FIN: 1 CON: 0 UNS: 0 SEQ: 2 FUNC: READ
ms(1440709784024) <-LL--  server     - Function: PRI_UNCONFIRMED_USER_DATA Dest: 10 Source: 1 Length: 17
ms(1440709784027) <-TL--  outstation - FIR: 1 FIN: 1 SEQ: 4 LEN: 11
ms(1440709784029) <-AL--  outstation - C3 14 3C 02 06 3C 03 06 3C 04 06
ms(1440709784031) <-AL--  outstation - FIR: 1 FIN: 1 CON: 0 UNS: 0 SEQ: 3 FUNC: ENABLE_UNSOLICITED
ms(1440709784034) <-AL--  outstation - 060,002 - Class Data - Class 1 - all objects
ms(1440709784036) <-AL--  outstation - 060,003 - Class Data - Class 2 - all objects
ms(1440709784039) <-AL--  outstation - 060,004 - Class Data - Class 3 - all objects
ms(1440709784041) --AL->  outstation - C3 81 00 00
ms(1440709784042) --AL->  outstation - FIR: 1 FIN: 1 CON: 0 UNS: 0 SEQ: 3 FUNC: RESPONSE IIN: [0x00, 0x00]
ms(1440709784046) --TL->  outstation - FIR: 1 FIN: 1 SEQ: 3 LEN: 4
ms(1440709784048) --LL->  outstation - Function: PRI_UNCONFIRMED_USER_DATA Dest: 1 Source: 10 Length: 5
ms(1440709784052) <-LL--  server     - Function: PRI_UNCONFIRMED_USER_DATA Dest: 10 Source: 1 Length: 20
ms(1440709784056) <-TL--  outstation - FIR: 1 FIN: 1 SEQ: 5 LEN: 14
ms(1440709784057) <-AL--  outstation - C4 01 3C 02 06 3C 03 06 3C 04 06 3C 01 06
ms(1440709784060) <-AL--  outstation - FIR: 1 FIN: 1 CON: 0 UNS: 0 SEQ: 4 FUNC: READ
```
