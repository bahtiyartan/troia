

========================
Working with FTP Servers
========================


Introduction
------------

Supported Protocols
===================

Secure File Transfer Protocol (also known as SFTP or SSH File Transfer Protocol), FTP (also known as File Transfer Protocol), FTPS (File Transfer Protocol SSL) are supported protocols.


Installing FTP Functionality
============================


Minimum interpreter release that supports FTP functionality is 5.01.04 041203, to use FTP functionality. If you have an older release, please update your interpreter to given or newer versions.

To enable FTP functionality following .jar files must be added to application servers’ class path:
	ftp4j-1.7.2.jar	: (to enable FTP/FTPS protocols, newer releases must be approved by R&D department)
	jsch-0.1.49.jar	: (to enable SFTP protocol, newer releases must be approved by R&D department)

If this jar files are not included by Application Server’s class path, application server does not support any FTP functionality, but other functionalities are not affected.  Due to requirements  it is possible to add one of this two jars to enable only required protocols such as FTP/FTPS or SFTP.
There is no need to add this jar files to client classpath, because all FTP processes are executed on server side.

Most possible problem about installation is missing required jar files. If jars are missing please TRACE your process and look for java.lang.NoClassDefFoundError on your trace file. SYS_STATUSERROR system symbol is another option to read java.lang.NoClassDefFoundError message. If you get this message please add required jars to Application Server’s class path and restart your application server.


Connection Management
---------------------

All FTP/FTPS/SFTP operations must be executed after connection established. MAKEFTPCONNECTION command is used to create connection. MAKEFTPCONNECTION command takes host, port, username and password to connect file server. Port parameter is optional and default ports are 21 for FTP, 990 for FTPS, 22 for SFTP. Also programmers must provide the connection protocol to MAKEFTPCONNECTON command. This option is not a symbol and must be selected on development phase. Protocol parameter is optional and default option is FTP.

Here is the syntax of the command:

::

	MAKEFTPCONNECTION HOST {host} [PORT {port}] [USERNAME {username}] 
	                                            [PASSW {password}][PROTOCOL {FTP|FTPS|SFTP}];

Every MAKEFTPCONNECTION creates a session between application server and file server. System allows creating multiple sessions to different file servers, but there is no need to create multiple sessions to same file server. Distinctive parameter between different servers is HOST parameter of connection, so system does not allow multiple connections to same host. Scope of connection management is execution context, so every transaction or TROIA component has its own connection pool.

TROIA programmer is responsible close all connections after operation finished. CLOSEFTPCONNECTION command is used to close connection which is established by MAKEFTPCONNECTION. Here is the syntax:

::
	
	CLOSEFTPCONNECTION {host}

Additionally system tries to kill remaining open connections before transaction close operation, but it is not recommended to leave ftp connections open after all ftp operations finished.

Working on a Directory
----------------------

After connection established to file server, a working directory is assigned to a client. So all commands executed in this path. Also, relative file paths are computed from this working directory.

To read, currently which directory are you working on please use, FTPRUNCOMMAND’s CURRENTDIRECTORY variation? Changing working directory is possible with FTPRUNCOMMAND’s CHANGEDIRECTORY variation. To get detailed information about this commands please read FTP Commands section.

Uploading & Download
--------------------

Uploading and downloading which are basic functionalities of FTP Infrastructure must be executed in session.  
All upload and download paths are computed relatively from working directory.
User permissions are an important issue, if uploading and downloading files fails firstly check user permissions. User information is about your connection which is established by MAKEFTPCONNECTION command. It is possible to execute multiple upload and download operations on a connection.
FTPUPLOAD, FTPDOWNLOAD commands are used to upload and download file. To get detailed information about this commands please read FTP Commands section.

Creating and Deleting Folders & Files
-------------------------------------

Infrastructure allows TROIA programmer to create and delete folders on working directory.  These operations are executed on a ftp connection which is established by MAKEFTPCONNECTION command.
To create and delete folders and delete files use FTPRUNCOMMAND command. To get detailed information about this command please read FTP Commands section.

Listing Files
-------------

FTP Infrastructure supports listing files. Operation is fired by FTPRUNCOMMAND command’s LISTFILE variation and executed as working directory. Result of this command must be assigned to a table symbol, similar to FILELIST command. To get detailed information about this command please read FTP Commands section.
