This section serves as a FAQ for common issues and their resolutions.

### Unable to find `asio.hpp`

You need to tell CMake where the ASIO headers are located. Please review the [CMake instructions](../build/cmake.md).

### Unable to load DNP3CLRAdapter or one of its dependencies

.NET does not provide useful error messages when an assembly can't find a native dependency. The .NET
bindings depend on the Visual C++ Runtime library for the version of Visual Studio you used to build them.

These packages can be found from Microsoft by searching for "Visual Studio C++ Runtime" in your favorite search engine.

You might also see this error because CLRAdapter can't find openssl (libeay32.dll / libssl32.dll) if your bindings didn't have
these linked statically. The solution is to make sure openssl is installed in the correct system location.

### My master and outstation don't communicate

The most common mistake when configuring a master to talk to an outstation is neglecting to set the correct addresses.
DNP3 was originally designed to communicate on a shared serial bus, so **components ignore messages with addresses
not intended for them**. Outstations expect to receive messages from a particular master, and will only respond to that
master if both the source and destination link-layer addresses are set correctly.

### Master or Outstation doesn't detect an ethernet cable being unplugged

There is no way through the standard socket API to detect this, other than trying to write data to the socket. This means that if
master or outstation sessions are quiescent (meaning that they don't write data periodically), you should be using the link-layer
[keep-alive feature](../api/linklayer.md#keep-alives) to ensure that dead/hung connections are appropriately detected.