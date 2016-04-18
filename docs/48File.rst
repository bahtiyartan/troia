

===============
File Operations
===============

*File operations, is one of the basic functionalities of programming languages. In TROIA programming language, all file operations are simplified and has extended features to help programmers. This sections aims to introduce basic file operations and other extended features to work on different file formats.*

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

	S1 = 'Temp Folder: ' + GETUSERINFO('client.temp.folder');
	S1 = S1 + TOCHAR(10) + 'File Sep: ' + GETUSERINFO('file.separator');
	S1 = S1 + TOCHAR(10) + 'User Home: ' + GETUSERINFO('user.home');
	S1 = S1 + TOCHAR(10) + 'User Dir: ' + GETUSERINFO('user.dir');
	S1 = S1 + TOCHAR(10) + 'Java Home: ' + GETUSERINFO('java.home');
	S1 = S1 + TOCHAR(10) + 'File Enc: ' + GETUSERINFO('file.encoding');
	S1 = S1 + TOCHAR(10) + 'SetGet Folder: ' + GETUSERINFO('user.setgetfolder');

	STRINGVAR3 = S1;
	
And here is the result of sample code, please run the code on your own client and see the difference:

::

	Temp Folder: C:\Users\btan.IASRDDC\RESOURCES\TMP\
	File Sep: \
	User Home: C:\Users\btan.IASRDDC
	User Dir: C:\603server\toWeb
	Java Home: C:\Program Files\Java\jre7
	File Enc: Cp1254
	SetGet Folder: C:\603server\RESOURCES\SETGET\LOCAL\IAS604502\btan\
	
	
Also its possible to read some useful information about server side to use on file operations. Server side information is provided by SYSGETSERVERINFO() method. Here is the sample code and its result for SYSGETSERVERINFO() method:

::

	OBJECT:
		STRING S1,
		STRING STRINGVAR3;

	S1 = 'File Sep: ' + SYSGETSERVERINFO('file.separator');
	S1 = S1 + TOCHAR(10) + 'Trace Folder: ' + SYSGETSERVERINFO('TraceFolder');

	STRINGVAR3 = S1;

::

	File Sep: \
	User Home: TRACES


Opening/Closing Files
---------------------

To open files, OPEN FILE command is used. Command takes file path and file open mode. File open mode shows the aim of opening file, such as creating new file, appending or reading. Default open mode is FORAPPEND. Here is the syntax to open files:

::

	OPEN FILE {file_path} [FORNEW|FORREAD|FORAPPEND] [FILEID { fileid }];
	
CLOSE FILE command closes the file. As default it takes no argument because it closes file which already opened. Here is the syntax:

::

	CLOSE FILE [FILEID { fileid }];

All basic file operations are performed on server side. But it is allowed to access client side files. System copies client side file to server and sends to client after the operation. If file transfer is not necessary system does not copy opened file. Here is the matrice that shows the operation due to file open modes:

+------------+------------------+------------------+------------------+
|            |   FORREAD        | FORAPPEND        | FORNEW           |
+------------+------------------+------------------+------------------+
|            | Copy file from   | Copy file from   |                  |
| OPEN FILE  | client to server | client to server | Just open file   |
|            | and open         | and open         |                  |
+------------+-------------------------------------+------------------+
|            |                  | Close file. Copy | Close file. Copy |
| CLOSE FILE | Just close file  | file from server | file from server |
|            |                  | to client        | to client.       |
+------------+-------------------------------------+------------------+


Working With Multiple Files
===========================

FILEID is optional argument for both OPEN FILE and CLOSE FILE commands. It defines a unique name for opened file. As default, system allow does not allow opening multiple files concurrently. If you programmers want to open another file before closing first one, he/she must be provide FILEID for each command. FILEID is a unique id and shows which file will be affected from the operation. If FILEID is not provided, system uses a defult file id.

..sample

Reading Files & Writing Files
-----------------------------

.

Reading Files
=============

.


Writing Files
=============

.




Copying Files
-------------

.
	

Other File & Director Operations
--------------------------------

Listing Files in a Directory
============================
.


Deleting Files
==============

.

Digesting Files
===============
.


File Compression
================

.

PDF File Operations
===================

.

Working With Images
-------------------
.

Sample 1: Reading & Writing Files
---------------------------------