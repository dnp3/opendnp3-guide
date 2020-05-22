# Frequently Asked Questions

### Unable to load DNP3CLRAdapter or one of its dependencies

.NET does not provide useful error messages when an assembly can't find a native
dependency. The .NET bindings depend on the Visual C++ Runtime library for the
version of Visual Studio you used to build them.

These packages can be found from Microsoft by searching for "Visual Studio C++
Runtime" in your favorite search engine.

You might also see this error because CLRAdapter can't find OpenSSL
(`libcrypto.dll` / `libssl.dll`) if your bindings didn't have these linked
statically. The solution is to make sure OpenSSL is installed in the correct
system location.

Note that older versions of OpenSSL (< 1.1.0) have DLLs with different names
(namely `libeay32.dll` and `ssleay32.dll`). These are **NOT** suitable for the
3.0 version of OpenDNP3. You need OpenSSL 1.1.1 or higher.

### My master and outstation don't communicate

The most common mistake when configuring a master to talk to an outstation is
neglecting to set the correct addresses. DNP3 was originally designed to
communicate on a shared serial bus, so **components ignore messages sent to
addresses other than their own**. Outstations expect to receive messages from a
particular master, and will only respond to that master if both the source and
destination link-layer addresses are set correctly.

### Master or Outstation doesn't detect an ethernet cable being unplugged

There is no way through the standard socket API to detect this, other than
trying to write data to the socket. This means that if master or outstation
sessions are quiescent (meaning that they don't write data periodically), you
should be using the link-layer [keep-alive
feature](../api/linklayer.md#keep-alives) to ensure that dead/hung connections
are appropriately detected.