==========================
Prerequisites on Windows
==========================

**Visual Studio 2013 Express (free)**

This ships with VC++11 and support for all the C++11 goodness. No plugins or anything are required and you can likely even use an express edition for C++.

**Boost**

Installing Boost is pretty easy once you have VS installed. You can actually use the installer from [BoostPro](http://www.boostpro.com/download/). Otherwise, you can build from source by following the directions [here](http://www.boost.org/doc/libs/1_53_0/more/getting_started/windows.html). Just be aware that you don't need all of boost if you want to save some time. When it comes time to run bjam.exe, your command line should look something like this:

.. code-block:: bat

   bjam.exe toolset=msvc --with-system --with-test --with-date_time


If you're doing a 64bit build, you can set this option with the "address-model=64" setting.

You can put the headers and libraries anywhere you like, but you must define two environment variables that Visual Studio will be looking for:

.. code-block:: bat
   
   BOOST_HOME = <path to the directory of the boost distribution>
   BOOST_LIB = <path where the boost libraries were built or deployed>


**Java (optional)**

If you don't want to build the java binding just unload the dnp3java project from your solution. If you do, set the JAVA_HOME environment variable so that the compiler can find the JNI includes.

