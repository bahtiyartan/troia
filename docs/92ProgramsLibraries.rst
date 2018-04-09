

=========================================
Shared Libraries & Running Other Programs
=========================================

*This sections aims to introduce using dll/so files as library to call methods in dll/so and invoking other programs to perform a specific action or open specific file formats.*


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

Users selects different default applications for specific file extensions, so it is not possible to indicate a correct application while writing TROIA code that opens a file in client side. For example; to show a .htm file, running chrome/internet expolorer is not a correct approach for a cross-platform application. The default browser (or another kind of application) depends on operating system of the client and user's choice.

To reduce creating a cross-platform solution, TROIA has a RUNFILE command that fires default application for given file extension. Using this command TROIA programmers totally ignore which application or program is default for given file, all operations are performed in interpreter layer.

::

	RUNFILE {filepathonclient};
	
RUNFILE command works only for client side paths, in other words it is not possible to fire default application in application server. Here is an example that creates a .htm file and runs it with default browser.

::

	OBJECT:
		STRING STRPATH;
	   
	STRPATH = '*C:\TMP\runfile.htm';
		
	OPEN FILE STRPATH FORNEW;
	PUT '<b>RUNFILE Command</b>';
	CLOSE FILE;

	RUNFILE STRPATH;

	
Same example, for a server side path.

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

	COMMAND = '*java -jar C:\TMP\OutPrinter.jar p1 p2 p3 p4';
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

	
Exercise 1: Checking domain availability 
----------------------------------------

::

	OBJECT: 
	 STRING STRINGVAR3,
	 STRING COMMAND,
	 STRING TMPRESULT;

	STRINGVAR1 = 'g';
	INTEGERVAR1 = 0;
	STRINGVAR3 = '';

	WHILE INTEGERVAR1  < 20 
	BEGIN
		INTEGERVAR1 = INTEGERVAR1 + 1;
		STRINGVAR1 = STRINGVAR1 + 'g';
		STRINGVAR2 = 'www.'+STRINGVAR1+'le.com';
		COMMAND = '*C:\TMP\whois.exe ' + STRINGVAR2;
		SYNCHRONIZE;
		RUNPROGRAM COMMAND WITH WAIT INTO TMPRESULT;

		IF STRPOS(TMPRESULT, 'No whois information found') > 0 THEN
			STRINGVAR3 = STRINGVAR3 + STRINGVAR2 + ' : available.';
		ELSE
			STRINGVAR3 = STRINGVAR3 + STRINGVAR2 + ' : not available.';
		ENDIF;

		STRINGVAR3 = STRINGVAR3 + TOCHAR(10);
	ENDWHILE;

