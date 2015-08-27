.. _testing-label:

=================================
Emphasis on testing
=================================

Opendnp3 aims to be a correct, robust, secure, and conformant implementation of the standard. To meet these goals, we do the following types of testing an analysis.

* **Unit testing** is the cornerstone of opendnp3. We follow the "write the test to prove the bug exists" approach to debugging and regression prevention. The test suite has over 400 individual tests for the core libraries.

* **Conformance testing** is a type of functional testing used to ensure we are compliant to the DNP3 standard. We have our own tools (external in Scala) that implement the test proceedures and we use at least one third-part test harness prior to every major release.

* **Static analysis** is an important technique for finding  defects in non-executing source code. We use Coverity Scan (free for OSS) and the Clang static analyzer to clean up any issues before releases.  We have also done occasional spot checks with PVS Studio.

* **Fuzzing** is an essential technique for finding input validation flaws.  We have reported more `defects <http://www.automatak.com/robus/>`_ in DNP3 implementations than any research group. The `Aegis Fuzzer <http://www.automatak.com/aegis/>`_ demonstrated the vulnerability of existing DNP3 implementations. We have also done spot checks with the tools from Codenomicon and Wurldtech.

* **Dynamic Analysis** is used to monitor running programs for defects. We use Valgrind in conjunction with our unit test suite and fuzzer before every release.
