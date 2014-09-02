===============================
Prerequisites
===============================

**C++11**

The core library is written in a lightweight subset of C++11. This makes the code more compact, easier to understand, and easier to maintain. 
You'll need a C++11 compiler like G++, Clang, or MSVC++.

**Standalone ASIO**

You'll need the header-only (non-BOOST) version of `ASIO <http://think-async.com/>`_ >= 1.10.2.  
Create an environment variable called ASIO_HOME and point it to the include sudirectory of the ASIO distribution, e.g.:

.. code-block:: bash

  ASIO_HOME = <path to 'include' directory of ASIO>