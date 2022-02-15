

===============
File Operations
===============

*File operations, is one of the basic functionalities of programming languages. In TROIA programming language, file operations are simplified and has extended features to help programmers. This sections aims to introduce basics of file operations and extended features.*

Introduction
------------

As mentioned before, TROIA platform has an multitier architecture. As a result of this architecture, some file operations are available in server, client or both client and server. Even if some file operations are available on client side, (as all TROIA commands) file commands run on server side and returns result to client. 

Client side paths has * character as a prefix to separate them from a server side path. So programmers must send file paths starting with * (star) character, to point a client side path.

::

	OBJECT:
		STRING STRPATH;
	
	/* client side path samples */
	STRPATH = '*C:\USERS\BTAN\Desktop\sourcefile.txt';
	STRPATH = '*C:\TMP\sourcefile.txt';
	
	/* server side paths */
	STRPATH = 'C:\TMP\sourcefile.txt';
	STRPATH = '\Files\sourcefile.txt';
	
Using relative paths starts from user.dir on client side, but it is not recommended to use relative paths on client side.Relative paths is more useful for server side to implement portable applications, it starts from the folder that application runs on.

GETUSERINFO() system function returns some useful information for file operations on client side such as file separator, temp folder etc. Here is some useful information, returned by GETUSERINFO method. For more information about the method plese see TROIA Help:

::

	OBJECT:
		STRING S1,
		STRING STRINGVAR3;

	S1 = '';
	S1 = S1 + TOCHAR(10) + GETUSERINFO('client.temp.folder');
	S1 = S1 + TOCHAR(10) + GETUSERINFO('client.setget.folder');			   //after 8.03 22.14.01-01
	S1 = S1 + TOCHAR(10) + GETUSERINFO('client.local.iot.scripts.folder'); //after 8.03 22.14.01-01
	S1 = S1 + TOCHAR(10) + GETUSERINFO('file.separator');
	S1 = S1 + TOCHAR(10) + SYSCLIENTFSEPARATOR; /* valid after 8.03 */
	S1 = S1 + TOCHAR(10) + GETUSERINFO('user.home');
	S1 = S1 + TOCHAR(10) + GETUSERINFO('user.dir');
	S1 = S1 + TOCHAR(10) + GETUSERINFO('java.home');
	S1 = S1 + TOCHAR(10) + GETUSERINFO('file.encoding');
	S1 = S1 + TOCHAR(10) + GETUSERINFO('user.setgetfolder');
	


	STRINGVAR3 = S1;
	
And here is the result of sample code, please run the code on your own client and see the difference:

::

	C:\Users\btan.IASRDDC\RESOURCES\TMP\
	C:\Users\btan.IASRDDC\RESOURCES\SETGET\LOCAL\803DEV\btan\
	C:\Users\btan.IASRDDC\RESOURCES\LOCALIOTSCRIPTS\LOCAL\803DEV\00\
	\
	\
	C:\Users\btan.IASRDDC
	C:\603server\toWeb
	C:\Program Files\Java\jre7
	Cp1254
	C:\603server\RESOURCES\SETGET\LOCAL\IAS604502\btan\
	
	
Also its possible to read some useful information about server side to use on file operations. Server side information is provided by SYSGETSERVERINFO() method. Here is the sample code and its result for SYSGETSERVERINFO() method:

::

	OBJECT:
		STRING SEP,
		STRING S1,
		STRING STRINGVAR3;

	SEP = SYSGETSERVERINFO('file.separator');
	
	S1 = S1 + TOCHAR(10) + SEP;
	S1 = S1 + TOCHAR(10) + SYSSERVERFSEPARATOR; /* valid after 8.03 */
	S1 = S1 + TOCHAR(10) + SYSGETSERVERINFO('TraceFolder');
	S1 = S1 + TOCHAR(10) + SYSGETSERVERINFO('ServerPath') + SEP + 'TempFiles' + SEP; 
	S1 = S1 + TOCHAR(10) + SYSGETSERVERINFO('server.temp.folder'); /*valid after 8.03.02 04xxxx*/

	STRINGVAR3 = S1;

::

	\
	\
	TRACES
	C:\Users\btan\workspace\troia\TempFiles\
	C:\Users\btan\workspace\troia\TempFiles\
	

Opening/Closing Files
---------------------

To open files, OPEN FILE command is used. Command takes file path and file open mode. File open mode shows the aim of opening file, such as creating new file, appending or reading. Default open mode is FORAPPEND. Here is the syntax to open files:

::

	OPEN FILE {file_path} [FORNEW|FORREAD|FORAPPEND] [FILEID { fileid }];
	
CLOSE FILE command closes the file. As default it takes no argument because it closes file which already opened. Here is the syntax:

::

	CLOSE FILE [FILEID { fileid }];

All basic file operations are performed on server side. But it is allowed to access client side files. System copies client side file to server and sends to client after the operation. If file transfer is not necessary due to open mode system does not copy opened file. On database transactions it is not recommended to access files on client side. Due to EnableDBTransactionUnityCheck configuration system may show an warning messages when accessing file in client side inside a database transaction. For more detail please see related sections. Here is the matrix that shows the operation due to file open modes:

+------------+------------------+------------------+------------------+
|            |   FORREAD        | FORAPPEND        | FORNEW           |
+------------+------------------+------------------+------------------+
|            | Copy file from   | Copy file from   |                  |
| OPEN FILE  | client to server | client to server | Just open file   |
|            | and open         | and open         |                  |
+------------+------------------+------------------+------------------+
|            |                  | Close file. Copy | Close file. Copy |
| CLOSE FILE | Just close file  | file from server | file from server |
|            |                  | to client        | to client.       |
+------------+------------------+------------------+------------------+

Both CLOSE FILE and OPEN FILE commands set SYS_STATUS to 1, if operation fails. Also SYS_STATUSERROR is set to error message. Here is an example that tries to read an unexisting file. Reading an unexisting file is an error, but appending to an unexisting file is not error. All file operations automatically creates required folders, so using unexisting folders is not an error (provided that user have required permissions). Here is two examples that shows successful and unsuccessful attempts of opening/closing files:

::

	OBJECT: 
		STRING STRPATH,
		STRING STRINGVAR3;

	STRPATH = '*C:\TMP\UnknownFile.txt';
	STRINGVAR3 = 'Open File: ';
	OPEN FILE STRPATH FORREAD;

	IF SYS_STATUS == 0 THEN
		STRINGVAR3 = STRINGVAR3 + 'successful' + TOCHAR(10);
	ELSE
		STRINGVAR3 = STRINGVAR3 + 'failed!' + TOCHAR(10);
	ENDIF;

	STRINGVAR3 =  STRINGVAR3 + 'Close File: ';
	CLOSE FILE;

	IF SYS_STATUS == 0 THEN
		STRINGVAR3 = STRINGVAR3 + 'successful' + TOCHAR(10);
	ELSE
		STRINGVAR3 = STRINGVAR3 + 'failed!' + TOCHAR(10);
	ENDIF;
	
::

	OBJECT: 
		STRING STRPATH,
		STRING STRINGVAR3;

	STRPATH = '*C:\TMP\NewFile.txt';
	STRINGVAR3 = 'Open File: ';
	OPEN FILE STRPATH FORNEW;

	IF SYS_STATUS == 0 THEN
		STRINGVAR3 = STRINGVAR3 + 'successful' + TOCHAR(10);
	ELSE
		STRINGVAR3 = STRINGVAR3 + 'failed!' + TOCHAR(10);
	ENDIF;

	STRINGVAR3 =  STRINGVAR3 + 'Close File: ';
	CLOSE FILE;

	IF SYS_STATUS == 0 THEN
		STRINGVAR3 = STRINGVAR3 + 'successful' + TOCHAR(10);
	ELSE
		STRINGVAR3 = STRINGVAR3 + 'failed!' + TOCHAR(10);
	ENDIF;
	
All open files must be closed by programmer, in other words; open files after file operations end are considered TROIA programming errors.


Working With Multiple Files
===========================

FILEID is optional argument for both OPEN FILE and CLOSE FILE commands. It defines a unique name for opened file. As default, system allow does not allow opening multiple files concurrently. Here is an invalid file operation, please try to find why this sample is an invalid logically.

::

	/* !!! Warning: This is an invalid code */
	OBJECT: 
		STRING STRPATH1,
		STRING STRPATH2;

	STRPATH1 = '*C:\TMP\NewFile1.txt';
	STRPATH2 = '*C:\TMP\NewFile2.txt';
	
	OPEN FILE STRPATH1 FORNEW;
	OPEN FILE STRPATH2 FORNEW;
	
	/* do something on files, part 1 */

	CLOSE FILE;
	
	/* do somehing on files, part 2 */
	
	CLOSE FILE;
	
If you programmers want to open another file before closing first one, they must be provide FILEID for each command. FILEID is a unique id and shows which file will be affected from the operation. If FILEID is not provided, system uses a defult file id. Correct code to open multiple files concurrently is below, in this example system is able to know which file will be closed on each close attempt.


::

	OBJECT: 
		STRING STRPATH1,
		STRING STRPATH2;

	STRPATH1 = '*C:\TMP\NewFile1.txt';
	STRPATH2 = '*C:\TMP\NewFile2.txt';
	
	OPEN FILE STRPATH1 FORNEW FILEID F1;
	OPEN FILE STRPATH2 FORNEW FILEID F2;
	
	/* do something on files, part 1 */

	CLOSE FILE FILEID F2;
	
	/* do somehing on files, part 2 */
	
	CLOSE FILE FILEID F1;
	
As it is obvious that each file access requires a FILEID parameter, to determine which file will be modified or read, so all file manipulation commands get FILEID parameter. Please focus on FILEID syntax in file related commands.


Writing Files & Reading Files
-----------------------------

Writing and reading are the most used and important file manipulation operations. Like other programming languages, before reading or writing files, file must be opened. 

Writing Files
=============

To write files PUT command is used. PUT supports encoding with CODEPAGE parameter, this encoding (utf-8, utf-16 etc) is used while converting given text to bytes. If encoding is not provided system uses ISO8859_9 as default encoding. Each PUT command adds a new line (\\n) and carriage return (\\r) character to the end of given parameters. To disable these two endline characters NODENDOFLINE optional parameter is used. 

::

	PUT {valuelist} [CODEPAGE {encoding}] [NOENDOFLINE] [FILEID {fileid}];

	
Here is a sample code which uses different variations of PUT command. Please check file content and compare with the code and its trace.

::

	OBJECT: 
		STRING STRPATH;

	OBJECT: 
		STRING STRVALUE;

	STRVALUE = 'This is ';
	STRPATH = '*C:\TMP\SourceFile3.txt';
	OPEN FILE STRPATH FORNEW;
	PUT 'This is first line';
	PUT STRVALUE, 'second line';
	PUT STRVALUE NOENDOFLINE;
	PUT 'third line';
	PUT STRVALUE, 'fifth line' CODEPAGE 'ISO8859_9';
	CLOSE FILE;

PUT command also has FILEID option to write files which have an id.
	
::

	OBJECT: 
		STRING STRPATH,
		STRING STRPATH2;

	STRPATH = '*C:\TMP\SourceFile4.txt';
	STRPATH2 = '*C:\TMP\SourceFile5.txt';

	OPEN FILE STRPATH FORNEW FILEID 'F1';
	OPEN FILE STRPATH2 FORNEW FILEID 'F2';

	PUT 'This is first file' FILEID 'F1';
	PUT 'This is second file' FILEID 'F2';

	CLOSE FILE FILEID 'F1';
	CLOSE FILE FILEID 'F2';


Reading Files
=============

::

	GET {variablelist} [CODEPAGE {encoding}] [FILEID {fileid}];

::

	OBJECT: 
		STRING STRPATH,
		STRING STRINGVAR1,
		STRING STRINGVAR3;

	STRINGVAR3 = '';
	STRPATH = '*C:\TMP\SourceFile3.txt';
	OPEN FILE STRPATH FORREAD;

	WHILE 1 
	BEGIN
		GET STRINGVAR1;

		IF STRINGVAR1 == '' THEN
			BREAK;
		ENDIF;

		STRINGVAR3 = STRINGVAR3 + STRINGVAR1 +  '/';
	ENDWHILE;

	CLOSE FILE;
	
::

	OBJECT: 
		STRING STRPATH,
		STRING STRPATH2;

	STRINGVAR3 = '';
	STRPATH = '*C:\TMP\SourceFile4.txt';
	STRPATH2 = '*C:\TMP\SourceFile5.txt';

	OPEN FILE STRPATH FORREAD FILEID 'F1';
	OPEN FILE STRPATH2 FORREAD FILEID 'F2';

	WHILE 1 
	BEGIN
		GET STRINGVAR1 FILEID 'F1';

		IF STRINGVAR1 == '' THEN
			BREAK;
		ENDIF;

		STRINGVAR3 = STRINGVAR3 + STRINGVAR1 +  '/';
	ENDWHILE;

	STRINGVAR3 = STRINGVAR3 + TOCHAR(10);

	WHILE 1 
	BEGIN
		GET STRINGVAR1 FILEID 'F2';

		IF STRINGVAR1 == '' THEN
			BREAK;
		ENDIF;

		STRINGVAR3 = STRINGVAR3 + STRINGVAR1 +  '/';
	ENDWHILE;

	CLOSE FILE FILEID 'F1';
	CLOSE FILE FILEID 'F2';
	
	
..getblock

::

	GETBLOCK {variable}, {delimiter} [CODEPAGE {encoding}] [FILEID {fileid}];
	
::

	OBJECT: 
		STRING STRPATH;

	STRPATH = '*C:\TMP\SourceFile3.txt';

	OPEN FILE STRPATH FORREAD;
	GETBLOCK STRINGVAR3,' is ';
	STRINGVAR3 = STRINGVAR3 + TOCHAR(10);
	CLOSE FILE;

::

	OBJECT: 
		STRING STRPATH;

	STRPATH = '*C:\TMP\SourceFile3.txt';

	OPEN FILE STRPATH FORREAD;
	GETBLOCK STRINGVAR3,'';
	STRINGVAR3 = STRINGVAR3 + TOCHAR(10);
	CLOSE FILE;




Copying Files
-------------

::

	OBJECT: 
		STRING STRSOURCEPATH,
		STRING STRDESTPATH;

	STRSOURCEPATH = '*C:\TMP\SourceFile3.txt';
	STRDESTPATH = '*C:\TMP\SourceFile3_Copy.txt';

	COPYFILE STRSOURCEPATH INTO STRDESTPATH;
	

Other File & Directory Operations
---------------------------------

Listing Files in a Directory
============================

::

	FILELIST {directorypath} TO {targettable};

::

	OBJECT: 
		STRING STRPATH;

	STRPATH = '*C:\TMP\';

	DESTROYTABLE TMPTABLE;
	FILELIST STRPATH TO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;
	
Checking File Existence
============================

To check whether a file exists or not, you must use FILEEXISTS command. This is the faster way of checking file existence. This command is supported on the versions after 5.02.05 062001.  

::

	FILEEXISTS {filepath};

::

	OBJECT: 
		STRING STRINGVAR1;

	STRPATH = '*C:\TMP\myfile.txt';

	FILEEXISTS STRINGVAR1;
	IF SYS_STATUS THEN
		STRINGVAR3 = 'File does not exist.';
	ELSE
		STRINGVAR3 = 'File exists.';
	ENDIF;


Deleting Files
==============

::

	DELTEFILE {filepath};

::

	OBJECT: 
		STRING STRPATH;

	STRPATH = '*C:\TMP\SourceFile3_Copy.txt';
	DELETEFILE STRPATH;


Digesting Files
===============

::

	DIGESTFILE {path} INTO {targetsymbol} [USING {hashingalgorithm}];

::

	OBJECT: 
		STRING STRPATH;

	STRPATH = '*C:\TMP\SourceFile3.txt';
	COPYFILE STRPATH INTO 'SourceFileOnServer.txt';
	DIGESTFILE 'SourceFileOnServer.txt' INTO STRINGVAR3 USING 'SHA1';
	DELETEFILE 'SourceFileOnServer.txt';

	DESTROYTABLE TMPTABLE;
	FILELIST '.' TO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;
	SORT TMPTABLE ON NAME;


File Compression
----------------

::

	ZIPFILE {filepath} INTO {targetzipfile};

::

	OBJECT: 
		STRING STRPATH,
		STRING STRPATH2;

	STRPATH = '*C:\TMP\SourceFile4.txt';
	STRPATH2 = '*C:\TMP\SourceFile5.txt';

	COPYFILE STRPATH INTO 'ServerSourceFile4.txt';
	COPYFILE STRPATH2 INTO 'ServerSourceFile5.txt';

	ZIPFILE 'ServerSourceFile4.txt|ServerSourceFile5.txt' INTO 'Demo.zip';

	COPYFILE 'Demo.zip' INTO '*C:\TMP\Demo.zip';
	DELETEFILE 'Demo.zip';
	
::

	UNZIPFILE {zipfilepath} INTO {targetdirectory};
	
::

	OBJECT: 
		STRING STRPATH,
		STRING STRPATH2;

	STRPATH = '*C:\TMP\UnzippedSourceFile4.txt';
	STRPATH2 = '*C:\TMP\UnzippedSourceFile5.txt';
	COPYFILE '*C:\TMP\Demo.zip' INTO 'Demo.zip';

	UNZIPFILE 'Demo.zip' INTO '.\';

	COPYFILE  'ServerSourceFile4.txt' INTO STRPATH;
	COPYFILE  'ServerSourceFile5.txt' INTO STRPATH2;

	DELETEFILE 'Demo.zip';
	

Example 1: Read Image in BASE64 Encoding
----------------------------------------

Here is a samplecode that reads a png file as base64 and builds a HTM document that shows this image as embedded image.

::

	/*read binary image*/
	OPEN FILE '*C:\TMP\sampleimage.png';
	GETBLOCK STRINGVAR3,'' CODEPAGE 'BASE64';
	CLOSE FILE;

	/*add html tags */
	STRINGVAR3 = '<img src="data:image/png;base64, ' + STRINGVAR3 + '"/>';

	/*write html file*/
	OPEN FILE '*C:\TMP\sampleimage.htm' FORNEW;
	PUT STRINGVAR3 CODEPAGE 'UTF-8';
	CLOSE FILE
	

Example 2: Writing Files
---------------------------------

Write a piece of TROIA code that:

	- Writes the list of files in a given folder to a file.

Example 3: Working With Multiple Files
-------------------------------------

Modify the code that you write for previous exercise and write a TROIA code that

	- opens a file
	- read blocks until the end of file.
	- write each word and its length to another file.
		format : word [4]
		         another [7]
				 and [3]
				 another [7]
	
Please be sure that your code opens two files concurrently.

Example 4: Prepare Zip Content
-----------------------------

	- Prepare two files that contains digest data (SHA1) of two files.
	- Create a zip file that contains 4 files.
	- Copy zip file to client's temporary folder.
	- Delete all temporary files in server.
	
	