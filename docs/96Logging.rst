

=========================================
Logging
=========================================

*TROIA Platform has an advanced logging infrastructure with various log options. This section aims to introduce all logging capabilities of the platform.*


Basics of Logging
=================

TROIA platform logs lots of user interactions from a login attempt to logout. This logs are stored on SYSIALOGS table on database that user uses while logging in and it can be also passed to 3rd party SIEM systems. 

SYSIASLOGS table contains too many details about user action, here is the list and details of the columns

::

	USERN			: username
	SESSIONID		: session id
	MACHINE			: client device
	TRANS			: transaction
	TRANSACTIONID	: transaction id
	SERVERID		: server id (this is the unique id for the connected application server)
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

