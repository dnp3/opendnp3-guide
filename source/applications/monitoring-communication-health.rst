Monitoring Communication health
===============================

Most of the time your master or outstation is communicating just fine with the outside world. Sometimes, however, there are conformance issues or mis-configurations that cause the two sides not to communicate. The most common case for this is when the link layer addresses are not set correctly. OpenDNP3 provides a coarse communication health callback for the stack that is different from the channel.

.. code-block:: cpp

   enum StackState {
           /// Online and communicating
	   SS_COMMS_UP = 0,
           /// Communication has been lost
	   SS_COMMS_DOWN = 1,
           /// Communication health is unknown
	   SS_UNKNOWN = 2
   };


Just call *AddStateListener* on your master or outstation interface. Binding a listener is done idiomatically for each language. You may bind a state listener at any time. All callbacks will come from the underlying thread pool and you will always receive the current state immediately after you bind the listener. You never have to 'ask' for the current state. In the following examples, assume that we already acquired a stack interface (master or outstation) from a channel. The callbacks here just print the state to console, but you can react to it however you like. You might display it to a user or write it to a database. It's important to remember, however, that your callbacks should be short. During the callback, the thread from the pool that runs that stack can't do any other work. If you need to do any kind of long running or blocking operation like writing the state to a database, it's best to marshal the event to a different thread.

.. code-block:: cpp

   // C++11 using a lambda
   pStack->AddStateListener([](StackState state){
     cout << "stack state: " << ConvertStackStateToString(state) << endl;
   });


.. code-block:: java

   // Java using an anonymous inner class
   stack.addStateListener(new StackStateListener() {
     @Override
     public void onStateChange(StackState state) {
       System.out.println("Stack state: " + state);
     }
   });


.. code-block:: csharp

   // c# using a lambda
   stack.AddStateListener(state => Console.WriteLine("Stack state: " + state));
