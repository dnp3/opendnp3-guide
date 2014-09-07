
=========================
Atmega 2560
=========================

The atmega 2560 chipset is an 8-bit AVR microcontroller. It has very limited resources:

.. code-block:: bash
  
  SRAM (Kbytes):  8
  Flash (Kbytes): 256Kbytes    
  Max. Operating Freq. (MHz):  16 MHz

The chipset is used in the popular `Arduino Mega <http://arduino.cc/en/Main/arduinoBoardMega>`_ hobby board. We do not use the Arduino toolchain or *Wiring* programming language in this example. The Arduino Mega is just a convenient evaluation board for testing opendnp3.

The example project is located in the **./embedded/atmelavr** subdirectory of the distribution. Atmel Studio >= 6.2 is required to open and compile the library. There are no other dependencies. You will also need a programmer like the AVRISP mkII to program the board.

The program is a good example of implementing the PAL for a microntroller. 

* Level 2 (nearly level 3) outstation with unsolicited support
* Dynamic memory allocation only during initialization
* Interrupt-driven hardware timer with a 10ms period to multiplexes the software timers
* Interrupt-driven non-blocking USART tx/tx 
* Makes use of the AVR's light sleep mode between interrupts
* Max APDU rx/t/x sizes turned down to 249 bytes
* Logging stripped to reduce size (-DOPENPAL_STRIP_LOGGING)

The demo doesn't do very much. It assigns a toggling binary value to one of five binary inputs every few seconds and reports the free amount of RAM.

You can also send LATCH_ON / LATCH_OFF control relay output block to index 0 and the board will set the PORTB LED.

