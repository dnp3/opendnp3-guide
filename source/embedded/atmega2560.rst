
=========================
Atmega 2560
=========================

The atmega 2560 chipset is an 8-bit AVR microcontroller. It has very limited resources:

.. code-block:: bash
  
  SRAM (Kbytes):  8
  Flash (Kbytes): 256Kbytes    
  Max. Operating Freq. (MHz):  16 MHz

The chipset is used in the popular `Arduino Mega <http://arduino.cc/en/Main/arduinoBoardMega>`_ hobby board. We do not use the Arduino toolchain or *Wiring* programming language in this example. The Arduino Mega is just a convienient evaluation board for testing opendnp3.

The example project is located in the **./embedded/atmelavr** subdirectory of the distribution. Atmel Studio >= 6.2 is required to open and compile the library. There are no other dependencies. You will also need a programmer like the AVRISP mkII to program the board.

The program is a good example of implementing the PAL for a microntroller. 

* Level 3 outstation with unsolicited support
* All static memory allocation
* Interrupt-driven hardware timer with a 10ms period to multiplexes the software timers
* Interrupt-driven non-blocking USART tx/tx 
* Makes use of the AVR's light sleep mode between interrupts
* Max APDU rx/t/x sizes turned down to 249 bytes (-DOPENDNP3_MAX_TX_APDU_SIZE=249, -DOPENDNP3_MAX_RX_APDU_SIZE=249)
* Logging stripped to reduce size (-DOPENPAL_STRIP_LOGGING)

Once built, you'll get the following output:

.. code-block:: bash

   Program Memory Usage 	:	137826 bytes   52.6 % Full
   Data Memory Usage 		:	4179 bytes     51.0 % Full

The demo doesn't do very much. It assigns a toggling binary value to one of five binary inputs every few seconds.

You can also send LATCH_ON / LATCH_OFF control relay output block to index 0 and the board will set the PORTB LED.

