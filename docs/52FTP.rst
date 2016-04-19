

========================
Working with FTP Servers
========================

*File operations, is one of the basic functionalities of programming languages. In TROIA programming language, file operations are simplified and has extended features to help programmers. This sections aims to introduce basics of file operations and extended features.*

Installation
------------

Installing FTP Functionality
============================

Minimum interpreter release that supports FTP functionality is 5.01.04 041203, to use FTP functionality. If you have an older release, please update your interpreter to given or newer versions.

To enable FTP functionality following .jar files must be added to application servers’ class path:
	ftp4j-1.7.2.jar	: (to enable FTP/FTPS protocols, newer releases must be approved by R&D department)
	jsch-0.1.49.jar	: (to enable SFTP protocol, newer releases must be approved by R&D department)

If this jar files are not included by Application Server’s class path, application server does not support any FTP functionality, but other functionalities are not affected.  Due to requirements  it is possible to add one of this two jars to enable only required protocols such as FTP/FTPS or SFTP.
There is no need to add this jar files to client classpath, because all FTP processes are executed on server side.


Supported Protocols
===================

Secure File Transfer Protocol (also known as SFTP or SSH File Transfer Protocol), FTP (also known as File Transfer Protocol), FTPS (File Transfer Protocol SSL) are supported protocols.


Possible Problems about Installation
====================================

Most possible problem about installation is missing required jar files. If jars are missing please TRACE your process and look for java.lang.NoClassDefFoundError on your trace file. SYS_STATUSERROR system symbol is another option to read java.lang.NoClassDefFoundError message. If you get this message please add required jars to Application Server’s class path and restart your application server.


Connection Management
---------------------

To download or upload a file from a file server firstly, a FTP/FTPS/SFTP session must be created. This session is created by MAKEFTPCONNECTION and closed by CLOSEFTPCONNECTION commands. All file transfer operations and other ftp commands must be executed between these two commands.

..

Every MAKEFTPCONNECTION creates a session between Application Server and file server. System allows creating multiple sessions to different file servers, but there is no need to create multiple sessions to same file server. Distinctive parameter between different servers is HOST parameter of connection, so system does not allow multiple connections to same host. Scope of connection management is ExecutionContext, so every Transaction or TROIA Component has its own connection pool.


Multiple file transfers are allowed on a single connection. TROIA programmer is responsible close all connections after operation finished. Additionally system tries to kill remaining open connections before transaction close operation.

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
