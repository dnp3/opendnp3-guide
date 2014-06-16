========================
DNP3 Introduction
========================

DNP3 (IEEE-1815, latest revision 2012) is a telemetry (aka SCADA) protocol. As such, it is primarily concerned with the reliable and efficient delivery of measurement data from an (aka slave) in the field to a utility master. Control requests, the canonical example being the operation of circuit breaker, are occasionally made from the master to outstation by an operator or an automated process. DNP3 can do other related tasks such as time synchronization, file transfer, etc. A full copy of the IEEE standard document is recommened as a reference for using this library. It is recommended that you at least scan a copy to familiarize yourself with the basic concepts.  This documentation is not a suitable replacement for a good working knowledge of the protocol.

**Compared to IEC 61850**

IEC61850 is an international standard for utility automation, whereas DNP3 is mostly deployed in North America and Australia.  When comparing the two, it is only useful to compare the telemetry / SCADA portions of 61850, as 61850 is actually a number of different related protocols and standards. Major differences include:

* IEC61850 includes standard object models as a layer above MMS. This has advantages and disadvantages. It enforces that vendors model equipment in the same way and this can reduce error prone configuration. The disadvantage is that it is not generic enough for all types of equipment. The accompanying standards that define how things like windmills are modeled have always lagged the core substation models. IEC61850 object models will not always be standardized in the face of innovation. As equipment on the power system evolves, the models must be updated and changed. DNP3 on the other hand doesn't make you model your device in any particular way. This can be bad for standardization, but makes it very flexible for use in any application.

* IEC61850 telemetry is ONLY suitable for transport over reliable connections like TCP. It does not define any error checking, instead preferring to rely on standard networking technologies. DNP3 can be used over a large cross-section of unreliable networks. 

**Compared to Modbus**

DNP3 has one major disadvantage vs Modbus. The entire Modbus standard is less than 70 pages and is easy to implement. The IEEE specification for DNP3 is in excess of 1,000 pages. It is hard to implement correctly. However, DNP3 has many advantages over Modbus that make it worth investing in a good implementation:

* Measurement data in dnp3 occurs in events. This means that an outstation is aware of which measurements a master has received and is able to only report the changes. This drastically reduces the amount of network traffic, especially for larger outstations. If a master ever loses synchronization it can always request the full static state.

* Unsolicited mode allows an outstation to "push" measurement data to a master as changes occur. Not only does this reduce network chatter, but it also reduces latency. A master doesn't have to scan an outstation at a high rate to maintain a high refresh rate for an operator console.

* DNP3 has a very strong Cyclic Redundancy Check (CRC) built into its lowest layer. This offers much better error resilience over lossy transport media than a check-sum.

* DNP3 is very scalable compared to Modbus. What is meant by this is that outstation databases can be of arbitrary size. In modbus, responses are limited to a certain size, whereas a dnp3 outstation can respond with a virtually unlimited number of message fragments that are individually parse-able.

* DNP3 has one profile that works well over a variety of transport media (aka physical layer). This means that a special flavor of the protocol isn't required for radio / mesh wireless / serial / TCP / UDP.
