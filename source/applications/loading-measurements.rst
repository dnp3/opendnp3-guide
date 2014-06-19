
======================================
Loading measurements
======================================

Measurement loading also uses *IDataObserver*, but in the opposite direction. Instead of receiving measurements, you'll be making the calls. You can obtain this interface from the interface you got when you created your outstation:

.. code-block:: cpp

   // C++
   IDataObserver* pDataObserver = pOutstation->GetDataObserver();

The IDataObserver interface is transactional. You must call Start() followed by 0 or more calls to Update(x) followed by a call to End(). The outstation will see this data as a batch, and so long as it can put all the measurement data into one fragment, the measurements will be reported to the master as a batch.

In C++ you can use the Transaction helper class. For the design pattern fans among you, this pattern is called the Resource Acquisition Is Initialization (`RAII <http://en.wikipedia.org/wiki/Resource_Acquisition_Is_Initialization>`_). This is a common pattern for writing code that guarantees a post condition.

.. code-block:: cpp

   // C++
   {
   Transaction t(pDataObserver); //automatically calls Start() on contruction
   pDataObserver->Update(Counter(3), 0); //default time and quality
   // .. update more measurements here
   } //automatically calls End() on destruction


Java and C# don't have deterministic destructors, but they do have finally blocks:

.. code-block:: csharp

   // C#
   publisher.Start();
   try {
     publisher.Update(new Analog(value, 1, DateTime.Now), 0);
   }
   finally {
     publisher.End();
   }


The DataObserver isn't going to throw and exception on you unless you try to load measurements that weren't configured in the outstation database.
