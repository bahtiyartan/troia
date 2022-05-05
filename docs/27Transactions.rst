

=============
Transactions
=============

A TROIA Transaction is simply an business level application that users can perform specific task/tasks on, like "Sales Document Management" or "Funds Management".

Transactions has a short name/id that is used as transaction id when starting/calling the transaction (DEVT11) and a caption which is a short text that defines transaction (Runcode Test Transaction).

Like other programming languages, TROIA transactions have an entry point that is fired when application started. This "entry point" is called as "start dialog" and it is defined on transaction definition.

Defining Transactions
---------------------

Before the definition, you must decide the id/name, the first dialog (entry point) and transaction caption. This three information is the primary data which defines a transaction.

Definition operation is performed on "SYST00 - System Transactions & Gadgets" application. This application is also a TROIA transaction. SYST00 stores transaction information on SYSTRANS, SYSTRANSTXT database tables.

Another feature that you must provide while defining a transaction is the "module" of your transaction. A module is virtual group of one or more transactions such as "Sales Applications" or "Finance Applications" etc. Module is not a primary data and relatively negliable, it is just used for documentation. Status (active/passive) information of transaction is also another data  just for docuementation.

While defining a transaction you can also provide icon, color for user interface issues.
	
	
Transactions and Scope
----------------------

Transaction is the only structure that is able to execute TROIA codes. In other words, to run a TROIA code at least one transaction must be opened in server side. Also, transactions are able to store variables defined in TROIA applications. A transaction is the largest scope of a TROIA variable. A variable which is defined as global, can be accessed by any dialog or class code which is running on transaction.


Running & Calling Transactions
------------------------------

When a transaction is starter, application server reads the basic information from transaction tables on database, and creates a virtual structure in server memory. This virtual structure is server side counterpart of transaction that will opened on client side. After a successful creation of server transaction, client calls start dialog of the transaction. After dialog's events performed, transaction gets ready to user interaction. 

Simply, there are two ways of starting a transaction. The first, most used and more natural one is running a transaction from client by clicking user menu, quick launch or a shortcut. The other method is called as "calling transaction" and used by programmers to run a transaction on TROIA code.


Calling Transactions by TROIA Code
==================================

In some cases, programmers may need call a transaction to perform a task and optionally get some output parameters from called transaction. "CALL TRANSACTION" command is used to call a transaction programatically.

Here is the simplest usages of CALL TRANSACTION command:

::

	CALL TRANSACTION {tran_name} [({input_params})];
	
	/* 
	   Example:
	   CALL TRANSACTION DEVT11 (PARAM1, PARAM2);
	*/
	
In this variation, TROIA runtime does not stop code execution on CALL TRANSACTION command. Just calls given transaction after server activity finished (when lastest event/method executed on user interaction.) In this case, getting output parameters from called transaction is not supported.


It is possible to stop code execution and wait for output parameters. Here is the usage:

::

	CALL TRANSACTION {tranname} [({input_params})] WITH WAIT [({output_params})];
	
	/* 
	   Example:
	   CALL TRANSACTION (PARAM1, PARAM2) DEVT11 (OUTPUT1, OUTPUT2);
	*/
	
In this case, TROIA runtime stops code execution and starts a new transaction that is interactive with user. When user closes the transaction, TROIA runtime continues code execution from the command after CALL TRANSACTION, so getting output parameters and use their values on next commands is possible. In WITH WAIT variation, users not allowed to switch caller transaction on user interface without called transaction is closed.


Calling Transactions in Server
==============================

As default CALL TRANSACTION command, fires opening a new transaction process drived by client, so new transaction is opened on client side with its user interface appears in client application. In this case,users are also able to interact with called transaction.

But in some cases, programmers may need result of a process which is already implemented on a transaction without any user interaction. In this cases, it is possible to call a transaction in server. When a transaction is called in server, server opens a transaction and it's first dialog, runs events and closes transaction; in other words server simulates all client requests which is called when a regular transaction opening process.

Calling a transaction with in server variation is very simple. Here is the syntax:

::

	CALL TRANSACTION {transaction} ({input_params}) INSERVER;
	CALL TRANSACTION {transaction} INSERVER ({input_params});
	CALL TRANSACTION {transaction} ({input_params}) INSERVER ({output_params});
	
Input Parameters & TRANSCALLED Event
--------------------------------------

Input parameters are defined on called transaction's as global variables with same name, value and type. So passing parameters as constant values is not recommended. 

::

	/* this is a valid call */
	CALL TRANSACTION DEVT11 ('paramvalue', 5);
	
	
If CALL TRANSACTION command has input parameters, after "AFTER" event of first dialog TRANSCALLED is called. If there is not any input parameter TRANSCALLED event is not fired. Implementing TRANSCALLED event in start dialog is not a compulsory. If dialog does not contain TRANSCALLED event, event call is ignored even if caller command passes parameter.
	

Scheduled Tasks and Batch Transactions
--------------------------------------

As mentioned before, transactions are the only structure that is able to execute TROIA codes. So when programmers needs to run a program as a planned/scheduled task,also they have to open a transaction. In TROIA platform, this case is called as "batch transaction" or "batch client".

Due to batch transaction concept, programmers are able to define a TROIA code block for a transction that will be configured as batch. This code is defined in "Batch Code" tab of "SYST00 - System Transactions & Gadgets" application. When a batch transaction is called, system starts transaction and opens it's first dialog as a regular transaction opening additionally runs transaction's "Batch Code".

To run a batch transaction, there are two methods. First one is client based batch and this method allows users to see client user interface. The other option is a console application which is defined as an Server Admin Utility variation. In both of two methods, scheduling is defined on operating system's scheduling tools like Window's task scheduler or linux's crontab (TROIA Platform does not have embedded scheduling tools).

For more configuration details about "batch transactions" please see help documents.


Reading Transaction Info
--------------------------------------

In some cases, as a programmer you may need some information about the transaction that your code runs in. Here are some usefuls system variables and functions that provides information about the transaction:

+-------------------------------+--------------------------------------------------------------------+
| SYS_TRANSACTION               | to get transaction name                                            |
+-------------------------------+--------------------------------------------------------------------+
| SYS_TRANSACTIONID             | to get the transaction id                                          |
+-------------------------------+--------------------------------------------------------------------+
| SYS_TRANSACTIONTYPE           | to get transaction type, is it a regular transaction or gadget?    |
+-------------------------------+--------------------------------------------------------------------+
| SYS_ISBATCHTRANSACTION        | to check whether transaction is batch or not                       |
+-------------------------------+--------------------------------------------------------------------+
| SYS_ISSERVERONLYTRANSACTION   | to check whether transaction is server only or not                 |
+-------------------------------+--------------------------------------------------------------------+
| SYS_TRANCREATEDBY             | to get the user who created the transaction                        |
+-------------------------------+--------------------------------------------------------------------+
| SYS_TRANCREATEDAT             | to get the time that transaction created                           |
+-------------------------------+--------------------------------------------------------------------+
| SYS_TRANMODIFIEDBY            | to get the last user who modified the transaction                  |
+-------------------------------+--------------------------------------------------------------------+
| SYS_TRANMODIFIEDAT            | to get the last time that transaction modified                     |
+-------------------------------+--------------------------------------------------------------------+

For more useful system variables and functions that returns data about transaction please see related help documents.


Exercise 1: Defining Transaction
------------------------------

Please define a transaction that

	- has a custom start dialog that has a textfield and a button on it.
	- calls DEVT11 transaction on button click event and returns STRINGVAR3 variable's value to textfield.

and try to understand difference between calling transaction with simple method and with "WITH WAIT" variation. 
	
	

