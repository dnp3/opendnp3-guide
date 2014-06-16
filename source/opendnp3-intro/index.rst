=========================
Opendnp3 Introduction
=========================

OpenDNP3 is a portable, scalable, and rigorously tested implementation of the DNP3 written in C++. The library 
is optimized for massively parallel front end processor implementations and slave device simulations, although it performs very well on embedded linux as well. Idiomatic bindings are available for the Microsoft CLR family of languages (C#, VB.NET, F#, etc) and JVM-based languages (Java, Scala, Clojure, etc).

The library has been in use production since 2006. This fork is maintained and supported by Automatak, LLC. The improvements here are based on many years of user feedback and real world deployment experience.

OpenDNP3 is very different from proprietary implementations. The biggest difference is the user has free access to the entire source code for the library and the associated tools like test-sets. You are free to experiment, change the code, etc, and can be as involved as you care to be in the future of the code-base. You have access to a community of experts whose collective experience and advice you can tap. You are not just the client of a company, although Automatak will provide as much support as you require.


.. toctree::
  :maxdepth: 2
  
  scalable-architecture
  control-types-and-modes
  no-microcontrollers
  where-is-the-super-loop

