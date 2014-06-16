=====================
Building on Windows 
=====================

If you've installed boost and set the three environment variables BOOST_INCLUDE, BOOST_LIB, and JAVA_HOME (optional) as described [[here|Prerequisites-on-windows]] the solution file should build without incident.

The default configuration on windows is to build the core C++ library as a static library. A shared library dll (opendnp3java.dll) is built for the Java bindings. .NET assemblies (also dll) are built for the .NET wrappers.
