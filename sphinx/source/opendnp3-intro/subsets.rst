===========================
Level Subset Support
===========================

IEEE-1815 defines 4 subset levels (1,2,3,4) that consist of the objects and function codes that must be supported by master and outstations to claim compliance.

Conformance tests only exist for subset levels 1 & 2. Opendnp3 is routinely tested for subset level 2 using 3rd party tools, but you can also configure the library to act as a simple levle 1 device.

A device profile template that describes the supported objects and function codes can be found on the `documentation landing page <http://dnp3.github.io>`_ .

**Level 3 Support (nearly complete)**

Notably missing is support for the ASSIGN_CLASS function code.

**Level 4 Support (partial)**

* No support for analog deadband objects (group 34)
* No support for command event objects (groups 13 & 43)
* No support for device attributes (group 0)

**Functionality not part of any subset**

* The master supports reading octet strings (Group 110/111), the outstation has no support for them.
* No support for virtual terminal objects
* No support for file transfer
* No support for datasets

**If your integration requires some functionality not currently implemented, consider sponsoring the additions**


    
  
  
