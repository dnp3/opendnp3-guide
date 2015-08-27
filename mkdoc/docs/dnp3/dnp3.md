### DNP3

DNP3 (IEEE-1815, latest revision 2012) is a standardized SCADA protocol with a large feature set.  This guide (or the opendnp3 project in general)
cannot teach you everything about DNP3. To succesfully use opendnp3 or devleop a product based on it, you will almost certainly need a copy of the specification. 
The opendnp3 project recommends that your organization joins [DNP.org](www.dnp.org) to obtain a copy.

Forget everything you know about Modbus. **DNP3 is event-oriented**. The master doesn't have to constantly scan the outstation for every single point. 
Instead it requests changes in one of two modes:

* **Event polling** - The master asks the outstations for changes on a regular interval (scan period). If nothing has changed, the response contains no measurement data. 
If the response contains event data, the master confirms them so that the data is not repeated in subsequent responses. This mode works well in multi-drop scenarios 
(like a 900mhz radio tower talking to many outstations), but is definitely more chatty as the master is still required to do some level of polling.

* **Unsolicited responses** - The outstation "pushes" events to the master as they occur, and the master confirms them. This mode works well with IP-based networks where 
multiple outstations can all talk to the master at the same time. You'll sometimes see this called "quiescient mode" since the network is silent when nothing is happening.

Understanding how DNP3 is event-oriented is the single most important concept to grasp. There are many other subtleties and complexities to discover as you implement DNP3 
software, but always keep this in mind.

### External Links

* [DNP Users Group](http://www.dnp.org)

* [DNP3 on Wikipedia](http://en.wikipedia.org/wiki/DNP3)

* [DNP3 Group on LinkedIn](https://www.linkedin.com/groups?home=&gid=1866115&trk=anet_ug_hm)