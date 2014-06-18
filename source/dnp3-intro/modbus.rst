======================
Compared to Modbus
======================

DNP3 has one major disadvantage vs Modbus. The entire Modbus standard is less than 70 pages and is easy to implement. The IEEE specification for DNP3 is in excess of 1,000 pages. It is hard to implement correctly. However, DNP3 has many advantages over Modbus that make it worth investing in a good implementation:

* Measurement data in dnp3 occurs in events. This means that an outstation is aware of which measurements a master has received and is able to only report the changes. This drastically reduces the amount of network traffic, especially for larger outstations. If a master ever loses synchronization it can always request the full static state.

* Unsolicited mode allows an outstation to "push" measurement data to a master as changes occur. Not only does this reduce network chatter, but it also reduces latency. A master doesn't have to scan an outstation at a high rate to maintain a high refresh rate for an operator console.

* DNP3 has a very strong Cyclic Redundancy Check (CRC) built into its lowest layer. This offers much better error resilience over lossy transport media than a check-sum.

* DNP3 is very scalable compared to Modbus. What is meant by this is that outstation databases can be of arbitrary size. In modbus, responses are limited to a certain size, whereas a dnp3 outstation can respond with a virtually unlimited number of message fragments that are individually parse-able.

* DNP3 has one profile that works well over a variety of transport media (aka physical layer). This means that a special flavor of the protocol isn't required for radio / mesh wireless / serial / TCP / UDP.
