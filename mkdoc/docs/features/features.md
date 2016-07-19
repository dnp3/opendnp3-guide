### Performance

Probably the single biggest 'feature' of the library isn't a DNP3 specific feature at all. Opendnp3 significantly outperforms
proprietary implementations of the protocol when it comes to large systems like masters with hundreds or even thousands of
concurrent sessions. There are a couple of reasons for this.

* Opendnp3 uses 100% non-blocking I/O and a thread-pool. There's no wasted context switching and thread-thrashing that plagues
thread-per-session systems.  The system also automatically shares sessions across the thread pool so that no artificial sharing is
required. Opendnp3 generally scales right up to your operating system limits.  We have the ASIO architecture to thank for
much of this as it automatically picks the most efficient event loop (select, kqueue, epoll, etc) for your platform.

* Zero-copy / zero-allocation parsing. When parsing an application layer message, the parser doesn't create a full object model
representation of the ASDU. Instead, it loops over the message in much the same way that a streaming XML parser does. This means
that messages can be parsed without heap allocation and very little stack usage as well.

### Robustness & Security

The principal developers who work on opendnp3 have lead the charge in the industry in terms of [security testing DNP3](https://www.automatak.com/robus).
You won't find a more reliable implementation of the protocol anywhere, even if you pay for it. Our commitment to a high-quality
project is evident in what we do.

* We have an exhaustive unit test suite in excess of 80% coverage
* We perform fuzzing using the [Aegis](https://www.automatak.com/aegis) smart fuzzer as well as [AFL](http://http://lcamtuf.coredump.cx/afl/).
* We do [automated static analysis](https://www.automatak.com/jenkins) using Coverity and cppcheck

We provide integrated TLS support that makes opendnp3 an ideal solution for integrating with real-time markets programs like
[PJM Jetstream](http://www.pjm.com/markets-and-operations/etools/jetstream.aspx).

### Compliance

IEEE-1815 defines 4 subset levels (1,2,3,4) that consist of the objects and function codes that must be supported by master and
outstations to claim compliance. A device profile template that describes the supported objects and function codes can be found
on the documentation landing page.

Conformance tests only exist for subset levels 1 & 2. Opendnp3 is routinely tested for subset level 2 using 3rd party tools, but
you can also configure the  library to act as a simple level 1 device. The stack currently meets all the level 2 subset requirements
with the notable except of support BROADCAST messages.  Very few people actually need this, and it's a questionable feature of the
protocol usually not recommended for use anymore.

 In addition to Level 2 support, the following features are supported, but not tested since there are no official tests for them currently.

**Levels 1-3** (FULL)

**Level 4** (PARTIAL)

* No support for analog dead-band objects (group 34)
* Only the master supports command event objects (groups 13 & 43)
* No support for device attributes (group 0)

**Functionality not part of any level subset**

* The master supports reading octet strings (Group 110/111), the outstation has no support for sending them.
* No support for virtual terminal objects
* No support for file transfer
* No support for data-sets
* No support for secure authentication

If your integration requires some functionality not currently implemented, consider sponsoring the additions
