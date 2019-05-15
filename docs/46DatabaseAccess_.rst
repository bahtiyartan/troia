

==================
Database Access
==================

As a business level programming language, TROIA has high collaboration with databases. With TROIA it is possible to perform too many operations on databases, such as connecting different databases, managing database transactions or executing sql queries. This section aims to introduce database operations and persistency flags of tables.

Database Connections of Session
-------------------------------

When a user logs in the system, system firstly finds database configuration from database section of server settings file using DBServer and DBName parameters from user's login parameters. This configuration contains reqired parameters to establish database connection. Application server establishes two different database connections to same database using this configuration. Therefore; in ordinary cases, TROIA programmers do not need to make any operation to connect database to run queries, it automatically gets ready for TROIA programmers use without any TROIA level effort. It is also same for closing these connections, it is totally handled by the interpreter and closed while user logging out.

This two connections are called "first db connection" and "second db connection" in TROIA jargon and has different purposes. First database connection is totally assigned to TROIA operations. In other words if your application contains a SELECT statement, system runs select query using first database connection. All TROIA level SQL commands uses first database connection. Second database connection is assigned for interpreter level database operations like inserting logs, gathering transaction's first dialog etc. 

Actually it is not possible to run data manipulation (SELECT,UPDATE,INSERT, DELETE) and data definition (CREATE, ALTER etc.) queries using second database connection, except some special troia commands like ENQUE, DEQUE. Therefore you can just ignore second database connection as a TROIA programmer.

As you may predict, database connection is a property or feature of user's session, so even if user opens more than one transaction all transactions uses same database connection. This means database connection is shared by all open transactions, and if a session changes state or a feature of connection this state/feature is valid for other transactions. Actually this is one of the main technical reasons of why it is not possible to run more than one processes on different transactions simultaneously. For some special cases like multithreading, it is possible to establish dedicated database connections for the transactions for more information please see the transactions and multithreading sections, but for now you can just ignore dedicated connections for transactions.

Briefly, system automatically establishes a database connection on user login to serve for all open TROIA applications and all SELECT, UPDATE, INSERT and DELETE commands uses this database connection.


Selecting data from Database
----------------------------
selecting data from database


Persistency Flags in Detail
----------------------------

All persistency flags are read-write, so its possible to set their values by code. All persistency flags are INTEGER (1 for true, 0 for false). For example: If DELETED flag is 1, it is deleted row  and programmer must send a delete query to database.

DELETED Flag
============
..

INSERTED Flag
=============
..

READ Flag
=========
..

UPDATED Flag
============
..

CHANGED Flag
============
..

CHECKED Flag
============
..


Updating Data on Database
-------------------------
..

EXECUTESQL Command
---------------
execute sql.

DB Transaction Management
-------------------------
db transaction management.


Fething Manually
----------------

selectline , fetch

Connecting Different Databases
------------------------------

connecting databases.


Defining Tables Using ODBA
--------------------------

odba.


Application Performance and Database
------------------------------------

performance.
