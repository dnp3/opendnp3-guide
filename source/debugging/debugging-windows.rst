=====================
Debugging on Windows
=====================

**Debugging .NET bindings using Visual Studio 2013**

When debugging the .NET assemblies some additional settings are required to debug into the C++ code. Without these set, Visual Studio may not be able to find the debug symbols and will not break at any breakpoints.

1. "Enable native code debugging" option is checked in Project Properties > Debug.
2. "Use Managed Compatibility Mode" option is checked in Tools > Options > Debugging > General.

See the following link for further details on `Managed Compatibility Mode
<http://blogs.msdn.com/b/visualstudioalm/archive/2013/10/16/switching-to-managed-compatibility-mode-in-visual-studio-2013.aspx>`_. 
Note that this setting is global and will affect all projects. An alternative is to add the DebugEngines property to the .csproj.user file as detailed in the link above.