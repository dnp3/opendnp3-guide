### Introduction

Welcome to opendnp3, a portable, rigorously-tested, Apache-licensed implementation of the DNP3 protocol (aka IEEE-1815).

### Important Links

* [Project Homepage](https://dnp3.github.io) - The official project homepage.

* [Github Repository](https://www.github.com/dnp3/opendnp3) - You'll find the tagged releases here as well as the development branches.

* [Google Group](https://groups.google.com/group/automatak-dnp3) - Post questions and receive updates. Help there is provided on a "as time permits" basis.

* [DNP Users Group](https://www.dnp.org) - The official DNP user group. Your organization should join and get a copy of the standard.

### Support options

If you need dedicated commercial support, custom integration services, or compliance/security testing contact [Automatak](http://www.automatak.com).

If the library does not contain a feature that you need for your device or application, please consider sponsoring the addition.


### About DNP3

DNP3 (IEEE-1815, latest revision 2012) is am open, standardized SCADA protocol with a large feature set.  This guide (or the opendnp3 project in general)
cannot teach you everything about DNP3. To successfully use opendnp3 or develop a product based on it, you will almost certainly need a copy of the specification.
The opendnp3 project recommends that your organization joins [DNP.org](https://www.dnp.org) to obtain a copy.

Forget everything you know about Modbus. **DNP3 is event-oriented**. The master doesn't have to constantly scan the outstation for every single point.
Instead it requests changes in one of two modes:

* **Event polling** - The master asks the outstations for changes on a regular interval (scan period). If nothing has changed, the response contains no measurement data.
If the response contains event data, the master confirms them so that the data is not repeated in subsequent responses. This mode works well in multi-drop scenarios
(like a 900mhz radio tower talking to many outstations), but is definitely more chatty as the master is still required to do some level of polling.

* **Unsolicited responses** - The outstation "pushes" events to the master as they occur, and the master confirms them. This mode works well with IP-based networks where
multiple outstations can all talk to the master at the same time. You'll sometimes see this called "quiescent mode" since the network is silent when nothing is happening.

Understanding how DNP3 is event-oriented is the single most important concept to grasp. There are many other subtleties and complexities to discover as you implement DNP3
software, but always keep this in mind.
