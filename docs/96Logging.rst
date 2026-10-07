

=========================================
Logging
=========================================

*TROIA Platform has an advanced logging infrastructure with various log options. This section aims to introduce all logging capabilities of the platform.*


Basics of Logging
=================

From an architectural perspective, the TROIA Platform involves two distinct logging domains. The first comprises logs related to a user's actions when attempting to connect to a specific database; examples include logging in, requesting to launch an application, or opening a document for viewing.

The second category consists of logs pertaining to the system's own infrastructure, rather than being directly linked to a specific database. Examples of such logs include server memory status and operations like shutting down or starting up the server.

This section focuses on the logs within the first domain—specifically those generated at the user level following a connection to a particular database.

Whether or not such logs are kept can be configured on a per-database basis. In other words, different logging strategies can be implemented for databases accessible via the same application server.


Log Content
-----------

This logs are stored on SYSIALOGS table on database that user requests to log in. It is also possible to pass these logs to 3rd party SIEM systems, we will discuss it under another title in this section.

The SYSIASLOGS table contains many details regarding user actions; here is the list of featured columns and their details.
::

	USERN			: username
	SESSIONID		: session id
	MACHINE			: client device
	TRANS			: transaction
	TRANSACTIONID	: transaction id
	SERVERID		: server id (the unique id for the connected app server)
	MESSAGE			: log content / log message
	LOGTOPIC		: lot topic
	LOGTIME			: log time as milliseconds
	CREATEDAT		: log time
	CREATEDBY		: username (similar to USERN column)

Log Topics
============


Reading Log Topics Programmatically
-----------------------------------

Log Configuration
-----------------


Writing Logs with TROIA
=======================

This command is avaliable on 26.10.06-01 and following builds.

::
	
	ADDLOGENTRY {message};
	ADDLOGENTRY {message} LOGLEVEL {loglevel};
	
::

	OBJECT:
		STRING LOGMESSAGE;
	
	LOGMESSAGE = 'invalid check table content';

	ADDLOGENTRY LOGMESSAGE LOGLEVEL 'ERROR';


Log Analyse Applications
========================

...log analysis application

SIEM Integration
----------------

...siem integration

