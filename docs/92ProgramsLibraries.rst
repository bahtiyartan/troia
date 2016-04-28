

=========================================
Shared Libraries & Running Other Programs
=========================================

*This sections aims to introduce using dll/so files as code library and invoking other programs to perform a specific action or open specific file formats.*


Introduction to Shared Libraries
--------------------------------

..

Using Shared Libraries in TROIA
-------------------------------

iasNativeCall.dll / iasNativeCall.so

LOADNATIVE Command
==================

::

	LOADNATIVE {libname} , {signature} TO {targetsymbol} WITH {parameters};

::

	OBJECT: 
		STRING STRINGVAR1,
		STRING STRINGVAR2;

	STRINGVAR1 = 'int abs(int)';
	LOADNATIVE '!msvcrt.dll',STRINGVAR1 TO STRINGVAR2 WITH '-4';
	
::

	OBJECT: 
		STRING STRINGVAR1,
		STRING STRINGVAR2;

	STRINGVAR1 = 'int abs(int)';
	LOADNATIVE 'msvcrt.dll',STRINGVAR1 TO STRINGVAR2 WITH '-4';


Opening Files with Default App
------------------------------

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

::

	RUNPROGRAM {executable} [params] [WITH WAIT [INTO {targetsymbol}]];


RUNPROGRAM Command
==================

::

	OBJECT:
		STRING STRINGVAR3,
		STRING COMMAND;
		
	COMMAND = '*ipconfig -all';
	RUNPROGRAM COMMAND WITH WAIT INTO STRINGVAR3;
	
	
::

	OBJECT:
		STRING STRINGVAR3,
		STRING COMMAND;

	COMMAND = '*java -jar C:\Users\btan.IASRDDC\Desktop\OutPrinter.jar p1 p2 p3 p4';
	RUNPROGRAM COMMAND WITH WAIT INTO STRINGVAR3;
	

.. code-block:: java

	public static void main(String[] args) {

	   System.out.println("Out Printer");
	   if (args.length > 0) {
	      for (int i = 0; i < args.length; i++) {
	         System.out.println("  Arg " + (i + 1) + ": " + args[i]);
	      }
	   } else {
	      System.out.println("  No Input Parameter.");
	   }
	}

	
Exercise 1:
------------------------------------

Exercise 2:
------------------------------------