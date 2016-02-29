

=======================
Transactions in Depth
=======================

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

Transaction is the only structure that is able to execute TROIA codes. In other words, to run a TROIA code at least one transaction must be opened in server side. Also, transactions are able to store store variables defined in TROIA applications. A transaction is the largest scope of a TROIA variable. A variable which is defined as global, can be accessed by any dialog or class code which is running on transaction.


Running & Calling Transactions
------------------------------

When a transaction is starter, application server reads the basic information from transaction tables on database, and creates a virtual structure in server memory. This virtual structure is server side counterpart of transaction that will opened on client side. After a successful creation of server transaction, client calls start dialog of the transaction. After dialog's events performed, transaction gets ready to user interaction. 

Simply, there are two ways of starting a transaction. The first, most used and more natural one is running a transaction from client by clicking user menu, quick launch or a shortcut. The other method is called as "calling transaction" and used by programmers to run a transaction on TROIA code.


Calling Transactions by TROIA Code
==================================
calling.


Scheduled Tasks and Batch Transactions
--------------------------------------
batch.

Sample 1: Defining Transaction
------------------------------
defining transaction.
	

