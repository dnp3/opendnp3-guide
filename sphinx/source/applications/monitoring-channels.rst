
======================================
Monitoring channels
======================================

Most of the time your communication channel is open and passing dnp3 traffic along. Sometimes, however, things can go wrong with your network or you have mis-configured your connection. The communication channel interface offers a way to monitor the state of channel. These states are represented by an enumeration:

.. code-block:: cpp

   enum class ChannelState : int
   {
     /// offline and idle
     CLOSED = 0,
     /// trying to open
     OPENING = 1,
     /// waiting to open
     WAITING = 2,
     /// open
     OPEN = 3,
     /// stopped and will never do anything again
     SHUTDOWN = 4
   };

**TODO - this secton needs work**


Binding a listener is done idiomatically for each language. You may bind a state listener at any time before or after a stack is added to channel. All callbacks will come from the underlying thread pool and you will always receive the current state immediately after you bind the listener. You never have to 'ask' for the current state. In the following examples, assume that we already acquired a channel interface (client, server, serial, etc) from the DNP3Manager root class. The callbacks here just print the state to console, but you can react to it however you like. You might display it to a user or write it to a database. It's important to remember, however, that your callbacks should be short. During the callback, the thread from the pool that runs that channel can't do any other work. If you need to do any kind of long running or blocking operation like writing the state to a database, it's best to marshal the event to a different thread.

.. code-block:: cpp

   // C++11 using a lambda
   pChannel->AddStateListener([](ChannelState state){
     cout << "state: " << ConvertChannelStateToString(state) << endl;
   });

