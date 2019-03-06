# an1310-python
A python implementation of the AN1310 PIC Bootloader

This is a simple python implementation of the AN1310 bootloader for PIC microcontrollers (see relevant application note from Microchip). The bootloader is used to program a PIC MCU through a serial port, most likely an USB-to-serial converter, provided you have the bootloader firmware already programmed in the PIC itself.

The program can be invoked with just the executable file as argument (in hex format) and it will try to autodetect the serial port (see below). Otherwise the you can pass the port name with `-p` parameter. Try `./an1310.py -h` for help.

Currently it only supports the PIC16F887, but see below for extensions. The procedure is as follows:
- assert break on serial port and ask user to reset the PIC
- connect to bootloader firmware
- write user program
- verify user program
- run user program
- run a serial terminal to interact with the user program through the same serial port

In the last step a serial terminal is started on the user's computer, using python package serial.tools.miniterm, so that you can exchange data with the user program, if needed, through the same connection, once the control is passed to the user program.

Limitations and how to extend it
================================

Currently only the PIC16F887 is supported. You can find a list of devices and their features in the `device_db` variable, in turn imported from the `devices.db` sqlite file distributed with the original AN1310 source code (see comments in the code). Other devices can easily be added in the same way.

The program only supports devices with auto-erase of flash blocks during writing. For other devices it should be easy to implement an erase routine in addition to write.

The autodetection of the serial port looks for an USB-backed port with VID/PID 0x0403,0x6015. This is an FTDI FT230X USB-to-serial converter, used in our specific board. You can easily change them or add a different search criterion.

Connection to bootloader firmware is attempted with a baud rate of 115200, while the serial terminal after the user program is run is set at 19200. You can easily change them.

Hints
=====

Be sure to have permissions to read and write the serial port device, and have the needed drivers installed. For all the rest, the same constraints as the original AN1310 program apply, so if it works there you should have no problems. 

The program depends on several python modules, see the imports at the beginning of the code.
