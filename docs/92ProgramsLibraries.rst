

=========================================
Shared Libraries & Running Other Programs
=========================================

*This sections aims to introduce using dll/so files as code library and invoking other programs to perform a specific action or open specific file formats.*


Introduction to Shared Libraries
--------------------------------

..


Using Shared Libraries in TROIA
-------------------------------

..

LOADNATIVE Command
==================

..


Opening Files with Default App
==============================

::

	RUNFILE {filepathonclient};
	
Client path..

::

	OBJECT:
		STRING STRPATH;
	   
	STRPATH = '*C:\TMP\runfile.htm';
		
	OPEN FILE STRPATH FORNEW;
	PUT '<b>RUNFILE Command</b>';
	CLOSE FILE;

	RUNFILE STRPATH;

	
Server path..

::

	OBJECT:
		STRING STRPATH;
	   
	STRPATH = '.\runfile.htm';
		
	OPEN FILE STRPATH FORNEW;
	PUT '<b>RUNFILE Command</b>';
	CLOSE FILE;

	RUNFILE STRPATH;
	DELETEFILE STRPATH;
	
Invoking Other Programs
-----------------------


RUNPROGRAM Command
==================

..