### Performance

Opendnp3 is engineered to perform exceptionally well for large systems with hundreds or even thousands of concurrent sessions:

* The protocol stack uses non-blocking I/O and runs on a thread-pool. This makes it extremely memory and CPU efficient at scale.

* Zero-copy / zero-allocation parsing. When parsing an application layer message, the parser doesn't create a full object model
representation of the message. This makes the processing of requests extremely fast and efficient.

### Robustness & Security

The principal developers who work on opendnp3 have lead the charge in the industry in terms of [security testing DNP3](https://www.automatak.com/robus).
You won't find a more reliable implementation of the protocol anywhere. Our commitment to a high-quality project is evident in what we do.

* An exhaustive unit test suite in excess of 80% coverage
* Fuzzing using the [Aegis](https://www.automatak.com/aegis) fuzzer and Google's OSS Fuzz
* Static analysis using tools like Coverity and cppcheck

We provide integrated TLS support that makes opendnp3 an ideal solution for integrating with real-time markets programs like
[PJM Jetstream](http://www.pjm.com/markets-and-operations/etools/jetstream.aspx).

### Compliance

IEEE-1815 defines 4 subset levels (1-4) that consist of the objects and function codes that must be supported by the master and
outstation. A device profile template that describes the supported objects and function codes can be found on the documentation
landing page.

Conformance tests only exist for subset levels 1 & 2. Opendnp3 is routinely tested for subset level 2 using 3rd party tools, but
you can also configure the  library to act as a simple level 1 device. The stack currently meets all the level 2 subset requirements
with the notable except of support BROADCAST messages.

#### Levels 1-4

| Level   | Support | Missing Features                              |
|---------|---------|-----------------------------------------------|
|    1    | FULL    |                                               |
|    2    | FULL    |                                               |
|    3    | FULL    |                                               |
|    4    | PARTIAL | Analog dead-bands (group 34), device attributes (group 0), command events (groups 13 & 43) |

#### Level 4+

| Feature                         | Support | Comment                                       |
|---------------------------------|---------|-----------------------------------------------|
| Octet strings (group 110/11)    | FULL    | Empty strings not allowed                     |
| Virtual Terminal                | NONE    |                                               |
| Secure Authentication           | NONE    | USE TLS!!!                                    |
| File Transfer                   | NONE    |                                               |
| Datasets                        | NONE    |                                               |

If your integration requires some functionality not currently implemented, consider sponsoring the additions.
