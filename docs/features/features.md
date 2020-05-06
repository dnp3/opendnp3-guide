### Performance

OpenDNP3 is engineered to perform exceptionally well for large systems with
hundreds or even thousands of concurrent sessions:

* The protocol stack uses non-blocking I/O and runs on a thread-pool. This makes
  it extremely memory and CPU efficient at scale.

* Zero-copy / zero-allocation parsing. When parsing an application layer
  message, the parser doesn't create a full object model representation of the
  message. This makes the processing of requests extremely fast and efficient.

### Robustness & Security

The principal developers who work on opendnp3 have lead the charge in the
industry in terms of [security testing DNP3](https://www.automatak.com/robus).
You won't find a more reliable implementation of the protocol anywhere. Our
commitment to a high-quality project is evident in what we do.

* An exhaustive unit test suite in excess of 80% coverage (excluding the
  generated files)
* Full [continuous integration](https://github.com/dnp3/opendnp3/actions) on
  Windows (64-bit and 32-bit) and Linux (using GCC and Clang)
* Fuzzing using the [Aegis](https://www.automatak.com/aegis) fuzzer and Google's
  [OSS Fuzz](https://github.com/google/oss-fuzz)
* Static analysis using tools like
  [LGTM](https://lgtm.com/projects/g/dnp3/opendnp3/context:cpp), Coverity and
  cppcheck

We provide integrated TLS support that makes opendnp3 an ideal solution for
integrating with real-time markets programs like [PJM
Jetstream](http://www.pjm.com/markets-and-operations/etools/jetstream.aspx).

### Compliance

IEEE-1815 defines 4 subset levels (1-4) that consist of the objects and function
codes that must be supported by the master and outstation. 

Conformance tests only exist for subset levels 1 & 2. The OpenDNP3 stack is
automatically tested against the latest DNP3 IED Certification Procedure (Subset
Level 2) **on every commit**. The detailed report with the associated packet
captures are available publicly. For the latest results, go
[here](https://github.com/dnp3/opendnp3/actions?query=branch%3Adevelop), click
on the latest commit, and download `conformance-results-{GIT_SHA}` file. The
device profile representing the virtual device used in these tests can be found
[here](https://github.com/dnp3/opendnp3/blob/develop/profile/opendnp3_profile.xml).

For information about the product used for automatic conformance testing and you
can use it to assess the conformity of your devices, contact
[Automatak](https://www.automatak.com/).

You can also configure the library to act as a Subset Level 1 device. The stack 
currently meets all the Subset Level 2 requirements.

#### Subset Levels 1-4

| Level   | Support | Missing Features                              |
|---------|---------|-----------------------------------------------|
|    1    | FULL    |                                               |
|    2    | FULL    |                                               |
|    3    | PARTIAL | Read/write some group 0 (Device Attributes), read g50v1 (Absolute Time and Date), read g80v1 (Internal Indications) |
|    4    | PARTIAL | Self-address, analog dead-bands (group 34), device attributes (group 0), command events (groups 13 & 43) |

#### Subset Level 4+

| Feature                         | Support | Comment                                       |
|---------------------------------|---------|-----------------------------------------------|
| Octet strings (group 110/11)    | FULL    | Empty strings not allowed                     |
| Virtual Terminal                | NONE    |                                               |
| Secure Authentication           | NONE    | Use TLS. See [this paper](https://www.cs.dartmouth.edu/~sergey/langsec/papers/crain-bratus-bolt-on-dnp3sa.pdf) for more details |
| File Transfer                   | NONE    |                                               |
| Datasets                        | NONE    |                                               |

If your integration requires some functionality not currently implemented,
consider sponsoring the additions.

### Portability

OpenDNP3 supports Windows and Linux operating systems. The continuous
integration pipeline validates that it compiles on both platforms with various
compilers.

Connections can be established using TCP/IP, a serial link, or a UDP connection.
Note that we do not recommend using UDP for DNP3 connections, and that serial
communications should only be used if TCP/IP is not available.

To ease usage in existing codebases, C# and Java bindings are also available.
They expose a complete and easy-to-use interface to interact with the library.
