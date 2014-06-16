Monitoring Channels
===================
Most of the time your communication channel is open and actively running dnp3 traffic. Sometimes, however, things can go wrong with your network or you have mis-configured your connection. The communication channel interface offers a way to monitor the state of channel. These states are represented by an enumeration:

.. code-block:: cpp

   enum ChannelState {
	   /// offline and idle
	   CS_CLOSED = 0,	
	   /// trying to open
	   CS_OPENING = 1,	
	   /// waiting to open
	   CS_WAITING = 2,	
	   /// open
	   CS_OPEN = 3,	
	   /// stopped and will never do anything again
	   CS_SHUTDOWN = 4	
   };


Binding a listener is done idiomatically for each language. You may bind a state listener at any time before or after a stack is added to channel. All callbacks will come from the underlying thread pool and you will always receive the current state immediately after you bind the listener. You never have to 'ask' for the current state. In the following examples, assume that we already acquired a channel interface (client, server, serial, etc) from the DNP3Manager root class. The callbacks here just print the state to console, but you can react to it however you like. You might display it to a user or write it to a database. It's important to remember, however, that your callbacks should be short. During the callback, the thread from the pool that runs that channel can't do any other work. If you need to do any kind of long running or blocking operation like writing the state to a database, it's best to marshal the event to a different thread.

.. code-block:: cpp

   // C++11 using a lambda
   pChannel->AddStateListener([](ChannelState state){
     cout << "state: " << ConvertChannelStateToString(state) << endl;
   });


.. code-block:: java

   // Java using an anonymous inner class
   channel.addStateListener(new ChannelStateListener() {
     @Override
     public void onStateChange(ChannelState state) {
       System.out.println("state: " + state);
     }
   });


.. code-block:: csharp

   // c# using a lambda
   channel.AddStateListener(state => Console.WriteLine("state: " + state));

