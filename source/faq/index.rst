
============================
Frequently Asked Questions
============================

.. links for the page
.. _change log: http://github.com/automatak/dnp3/blob/2.0.x/CHANGELOG.markdown
.. _Apache Version 2: http://www.apache.org/licenses/LICENSE-2.0.html
.. _ASIO: http://think-async.com/Asio/asio-1.10.2/doc/asio/using.html

**Which is the license for the library? Can I use it on closed source projects?**

All source code in the main repository is licensed under the `Apache Version 2`_. You may use the library in closed-source derivative works, however, you must still honor the minimal requirements of the license. For example, when you redistribute your derivative work it must contain a copy of the license and a notification.

**Has opendnp3 been tested for compliance?**

The outstation is tested using the latest level 2 conformance procedures before every release. This involves our own test suite and at least one 3rd party test harness. 
Be advised that only end-products are allowed to claim conformance on the DNP.org website. If this listing is critical to selling your products you have three options for testing:

* Self-test (results must be made available to DNP3 technical committee)
* Hire Automatak to test your integrated product (we will use a 3rd party tool)
* Hire a 3rd party to perform this testing

**Which DNP3-compliant systems has been opendnp3 been shown to work with?**

Too many to name, but the list includes products from SEL, GE, ABB, Novatech, Alstom. We have also used the simulation tools from TMW and ASE. 
Opendnp3 users have found and reported non-conformant behavior in 3rd party implementations much more frequently than issues in the opendnp3 stack.

**Which are the differences between the 1.1.x and the 2.0.x series? Are both completely functional DNP3 implementations?**

Yes, both are functional. The many differences are recorded in the `change log`_. The biggest differences are:

* The outstation library only performs dynamic allocation during initialization, and makes no use of any features of the C++ std library or STL.
* There is a Platform Abstraction Layer (PAL) that allows for ports to platforms other than those supported by ASIO.
* The dnp3 server wrapper library (asiodnp3) uses the header-only version of ASIO and does not require the BOOST C++ libraries.
* The tests have been ported to Catch++
* The library has a faster and safer application layer parsing mechanism

**The project is written in C++ but there are bindings for C#. Which languages/IDEs/Operating System do you encourage to use when developing an opendnp3 application?**

This information is covered in the :ref:`platforms-label` section.





