

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
	


Opening/Closing Files
---------------------

.


Reading Files & Writing Files
-----------------------------

.

Reading Files
=============

.


Writing Files
=============

.

Working With Multiple Files
===========================

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