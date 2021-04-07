

=================================
Port Communication (Serial & TCP)
=================================

*TROIA Platform supports sending/recieving data over serial ports. Also its possible to communicate over TCP ports using TCP Protocol. This sections aims to introduce communicating over serial and TCP ports*


Introduction
------------

Opening/Closing Serial Ports
----------------------------

..serial port

::

	OPENPORT {port} {portname} [ PARAMETERS [BAUDRATE {baudrate}] 
				[DATABITS {databits}] 
				[STOPBITS {stopbits}] 
				[PARITY NONE|ODD|EVEN|MARK|SPACE] 
				[FLOWCONTROL NONE|RTSCTSIN|RTSCTSOUT|XONXOFFIN|XONXOFFOUT] 
				[RTS {rts}] 
				[DTR {dtr}] 
				[ENCODING {encoding}] 
				[DEBUG TRUE|FALSE] ];
					
::

	CLOSEPORT {portname};
	

Open/Close COM1 port on client:
	
::

	OBJECT: 
		STRING PORT,
		STRING PORTNAME;

	PORTNAME = '*PORT';
	PORT = 'COM1';
	OPENPORT PORT PORTNAME PARAMETERS BAUDRATE '9600'
						DATABITS '8' STOPBITS '1' PARITY NONE;

	CLOSEPORT PORTNAME;


Opening/Closing TCP Ports
-------------------------

::

	OPENPORT TCP {portname} PORT {portnumber} [IPADDRESS {ipaddress}] [ENCODING {encoding}];
	

::

	CLOSEPORT {portname};
	

open a tcp port to 192.168.5.114
	
::

	OBJECT:
		STRING PORTNAME;

	PORTNAME = '*PORTNAME';
	OPENPORT TCP PORTNAME PORT 4848 IPADDRESS '192.168.5.114';
	CLOSEPORT PORTNAME;


open a tcp port on localhost

::

	OBJECT:
		STRING PORTNAME;

	PORTNAME = '*PORTNAME';
	OPENPORT TCP PORTNAME PORT 4848;
	
	CLOSEPORT PORTNAME;


Reading Data From Port
----------------------

::

	READFROMPORT {portname} INTO {variable};
	

read from a serial port.	

::

	OBJECT: 
		STRING PORT,
		STRING PORTNAME
		STRING RESULT;

	PORTNAME = '*PORT';
	PORT = 'COM1';
	OPENPORT PORT PORTNAME PARAMETERS BAUDRATE '9600' 
						DATABITS '8' STOPBITS '1' PARITY NONE;
	INTEGERVAR1 = 0;
	STRINGVAR3 = '';
	RESULT  = '' ;

	WHILE INTEGERVAR1 < 100 
	BEGIN
		READFROMPORT PORTNAME INTO RESULT;
		IF RESULT != '' THEN
			STRINGVAR3 = RESULT;
		ENDIF;
		DELAY 100;
		INTEGERVAR1 = INTEGERVAR1 + 1;
	ENDWHILE;

	CLOSEPORT PORTNAME;	

	
read data from a tcp port

::

	OBJECT:
		STRING STRINGVAR3,
		STRING PORTNAME;

	PORTNAME = '*PORTNAME';
	OPENPORT TCP PORTNAME PORT 4848 IPADDRESS '192.168.5.114';
	READFROMPORT PORTNAME TO STRINGVAR3;
	CLOSEPORT PORTNAME;	
	
Writing Data To Port
--------------------

::
	
	SENDTOPORT {portname} {content} [{timetowait}];
	

write data to a tcp port

::

	OBJECT:
		STRING PORTNAME;

	PORTNAME = '*PORTNAME';
	OPENPORT TCP PORTNAME PORT 4848;
	SENDTOPORT PORTNAME 'Hello World!';
	CLOSEPORT PORTNAME;
	
	

Example 1: Transfering File through Socket
------------------------------------------

This example transfers a single line of a file between two canias client through a TCP connection.

Here is the code for transmitter, this transmitter reads the entire file and pass it to receiver.

::

	OPEN FILE '*C:\TMP\tcp-example\korean.txt';
	GETBLOCK STRINGVAR3,'' CODEPAGE 'ISO8859_9';
	CLOSE FILE;
	
	OBJECT: 
		STRING PORTNAME;

	PORTNAME = '*PORT1';
	OPENPORT TCP PORTNAME PORT 50507 IPADDRESS '192.168.5.164' ENCODING 'ISO8859_9';
	STRINGVAR1 = SYS_STATUS;
	SENDTOPORT PORTNAME STRINGVAR3;
	STRINGVAR2 = SYS_STATUS;
	CLOSEPORT PORTNAME;
	
	
	
And the code for the receiver, this receiver reads a single line and breaks tcp connection, then writes the line to target file.

::

	OBJECT: 
		STRING PORTNAME;

	PORTNAME = '*PORT1';
	OPENPORT TCP PORTNAME PORT 50507 ENCODING 'ISO8859_9';

	STRINGVAR3 = '';
	WHILE 1 == 1 
	BEGIN
		READFROMPORT PORTNAME INTO STRINGVAR3;

		IF STRINGVAR3 != '' THEN
			BREAK;
		ENDIF;
	ENDWHILE;
	CLOSEPORT PORTNAME;

	OPEN FILE '*C:\TMP\tcp-example\target.txt' FORNEW;
	PUT STRINGVAR3,'' CODEPAGE 'ISO8859_9';
	CLOSE FILE;





	