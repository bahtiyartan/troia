

=================================
Port Communication (Serial & TCP)
=================================

*TROIA Platform supports port communications over serial ports. Also its possible to communicate over TCP Ports using TCP Protocol. This sections aims to introduce communicating over serial and TCP ports*


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

	OPENPORT TCP {portname} PORT {portnumber} [IPADDRESS {ipaddress}];

Reading Data From Port
----------------------

::

	READFROMPORT {portname} INTO {variable};
	
::

	OBJECT: 
		STRING PORT,
		STRING PORTNAME
		STRING RESULT;

	PORTNAME = '*PORT';
	PORT = 'COM1';
	OPENPORT PORT PORTNAME PARAMETERS BAUDRATE '9600' DATABITS '8' STOPBITS '1' PARITY NONE;
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

Writing Data To Port
--------------------

::
	
	SENDTOPORT {portname} {content} [{timetowait}];
	






	