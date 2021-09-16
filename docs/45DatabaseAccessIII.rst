

====================
Database Connections
====================

TROIA Platform establishes a database connection while logging in to manage TROIA level database operations. Moreover, it is possible to establish and manage multiple database connections for multi-database operations such as data transfer between different database systems with TROIA. This section aims to explain how to work with multiple databases and manage db transactions.


Basics of DB Connection In TROIA
--------------------------------

---------------------------------
Database Configurations and Login
---------------------------------

When a user logs in the system, system firstly finds database configuration from database section of server settings file using DBServer and DBName parameters from user's login parameters. This configuration contains required parameters to establish database connection. Application server establishes two different database connections to same database using this configuration. Therefore; in ordinary cases, TROIA programmers do not need to make any operation to connect database to run queries, it automatically gets ready for TROIA programmers use without any TROIA level effort. It is also same for closing these connections, it is totally handled by the interpreter and closed while user logging out.


.. figure:: images/database/db-connection.png
   :width: 574 px
   :target: images/platformbasics/db-connection.png
   :align: center


This two connections are called "first database connection" and "second database connection" in TROIA jargon and they have different purposes. First database connection is assigned to TROIA operations. In other words if your application contains a SELECT statement, system runs select query using first database connection. All TROIA level SQL commands uses first database connection. Second database connection is assigned for interpreter level database operations like inserting logs, gathering transaction's first dialog etc. 

Actually it is not possible to run data manipulation (SELECT, UPDATE, INSERT, DELETE) and data definition (CREATE, ALTER etc.) queries using second database connection, except some special troia commands like ENQUE, DEQUE. Therefore you can just ignore second database connection as a TROIA programmer.



As you may predict, database connection is a property or feature of user's session, so even if user opens more than one transaction all transactions uses same database connection. This means database connection is shared by all open transactions, and if a session changes state or a feature of connection this state/feature is valid for other transactions. Actually this is one of the main technical reasons of why it is not possible to run more than one processes on different transactions simultaneously. For some special cases like multithreading, it is possible to establish dedicated database connections for each transaction. We will discuss this advanced issue in next sections, but for now you can just ignore dedicated database connections for transactions.

Briefly, system automatically establishes a database connection on user login to serve for all open TROIA applications and all SELECT, UPDATE, INSERT and DELETE commands uses this database connection.




Connecting Different Databases
------------------------------

connecting databases.


Dedicated Database Connections for Transactions
-----------------------------------------------

...


SQL System Variable & Creating Scripts
--------------------------------------

..


