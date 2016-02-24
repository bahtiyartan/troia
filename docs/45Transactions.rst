

=======================
Transactions in Depth
=======================

A TROIA Transaction is simply an business level application that users can perform specific task/tasks on, like "Sales Document Management" or "Funds Management".

Transactions has a short name/id that is used as transaction id when starting/calling the transaction (DEVT11) and a caption which is a short text that defines transaction (Runcode Test Transaction).

Like other programming languages, TROIA transactions have an entry point that is fired when application started. This entry point is the start (first) dialog and this dialog is defined while transaction defined.

Defining Transactions
---------------------

Before the definition, you must decide the id/name, the first dialog (entry point) and transaction caption. This three information is the primary information which defines a transaction.

Definition operation is performed on "SYST00 - System Transactions & Gadgets" application. This application is also a TROIA transaction and stores provided information on SYSTRANS, SYSTRANSTXT database tables.

Another feature that you must provide while defining a transaction is the "module" of your transaction. A module is virtual group of one or more transactions such as "Sales Applications" or "Finance Applications" etc. Module is not a primary data and relatively negliable, it is just used for documentation. Status (active/passive) information of transaction is also another data  just for docuementation.

While defining a transaction you can also provide icon, color for user interface issues.
	
	
Transactions and Scope
--------------------

scope...


Running & Calling Transactions
------------------------------

Running Transactions
====================
running


Calling Transactions from Another Transaction
=============================================
calling
	

