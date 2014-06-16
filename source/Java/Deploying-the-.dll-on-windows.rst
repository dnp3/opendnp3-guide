Deploying the .dll on windows
-----------------------------

Use Visual Studio to produce a release build of java library. Remember that JAVA_HOME has to be defined to correctly include the JNI headers.

The windows version of the java bindings is a standalone dll called opendnp3java.dll. Copy the file to anywhere on your java.library.path. By default this is going to be your Windows/system32 or Windows/SysWOW64 if you using 64-bit windows

Running the maven tests from the shell is the same as for Linux.
