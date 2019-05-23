The Java bindings are built as two separate pieces:

* standard JAR file
* native shared library built from the C++ source

!!! important 
    Java 1.8 or greater is required because of features used in Java bindings

### Building the C++ bindings

Use the '-DDNP3_JAVA=ON' option when configuring CMake. CMake will locate your JNI headers using the JAVA_HOME environment variable. The shared
library must be installed in a system location or the path specified using the `system.library.path` JVM option.


### Building the JAR

The java library is built and installed locally using Maven.

```
> cd java
> mvn install
```

You can now use it in your Maven based Java projects using 

```xml
<dependencies>
...
	<dependency>
		<groupId>com.automatak.dnp3</groupId>
		<artifactId>opendnp3-bindings</artifactId>
		<version>${opendnp3.version}</version>
	</dependency>
...
</dependencies>
```
