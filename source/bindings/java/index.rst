Java Support
============

.. toctree::
   :maxdepth: 2
   :glob:

   deploying-the-dll-on-windows
   deploying-the-so-on-linux   


Java support is accomplished via a technology called Java Native Interface (JNI). You don't really need to know anything about JNI to use the library from Java, but it is good to understand a little about what's going on so you can properly build and deploy it.

There are two components to the java bindings. The java part will look and feel just like the Java that you're used to.  Once you've built the C++ java wrapper library, you can built and install this Jar with maven:

.. code-block:: bash

  > cd java
  > mvn install

This step will run the java tests and install the JAR into your local mvn repo.

With the Jar and C++ libraries installed you can now reference the opendnp3 java library within a maven POM:

.. code-block:: xml

   <dependencies>
           <dependency>
               <groupId>com.automatak.dnp3</groupId>
               <artifactId>opendnp3-bindings</artifactId>
               <version>1.1.0-RC1</version>
               <scope>compile</scope>
           </dependency>
   </dependencies>


Keep in mind that the version number above will change as new releases occur.

The second part of the bindings is a native shared library that implements some of the functions marked 'native' in the java source code. Think of it like an interface that the cpp code has to meet. You really never want to have to learn this technology. It works, it performs, but it's not fun to write. At run-time, a static initializer is called to load the library before any of the native methods can be called.

.. code-block:: java

   static {
     if(System.getProperty("com.automatak.dnp3.nostaticload") == null)
     {
       System.loadLibrary("opendnp3java");
     }
   }


Notice that there's a property that you can set to suppress the static loading.  This is important for OSGi containers as you'll need to load the library on the right class-loader. Most users will never have to worry about this detail, but having an override can be important.
