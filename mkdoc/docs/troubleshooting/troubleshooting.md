
### Common Problems

This section serves as a bit of a FAQ for common issues and their resolutions.

**Unable to find "asio.hpp"**

You need to tell CMake where the ASIO headers are located. Please review the [cmake instructions](../build/cmake.md).

**Unable to load DNP3CLRAdapter or one of its dependencies**

.NET does not provide useful error messages when an assembly can't find a native dependency. The .NET
bindings depend on the Visual C++ Runtime library for the version of visual studio you used to build them.

These packages can be found from Microsoft by searching for "visual studio c++ runtime" in your favorite search engine.

You might also see this error because CLRAdapter can't find openssl (libeay32.dll / libssl32.dll) if your bindings didn't have
these linked statically. The solution is to make sure openssl is installed in the correct system location.