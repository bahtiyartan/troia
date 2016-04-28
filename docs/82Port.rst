

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

Opening/Closing TCP Ports
-------------------------

::

	OPENPORT TCP {portname} PORT {portnumber} [IPADDRESS {ipaddress}];

Reading Data From Port
----------------------

::

	READFROMPORT {portname} INTO {variable};

Writing Data To Port
--------------------

::
	
	SENDTOPORT {portname} {content} [{timetowait}];
	






	