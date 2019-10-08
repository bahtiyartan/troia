

==================
Database Access
==================

As a business level programming language, TROIA has high collaboration with databases. With TROIA it is possible to perform too many operations on databases, such as connecting different databases, managing database transactions or executing sql queries. This section aims to introduce database operations and persistency flags of tables.



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

Updating data...


Inserting data...


Deleting data...


EXECUTESQL Command
------------------
execute sql.



DB Transaction Management
-------------------------
db transaction management.



Connecting Different Databases
------------------------------

connecting databases.


Defining Tables Using ODBA
--------------------------

odba.


Dedicated Database Connections for Transactions
-----------------------------------------------

...


SQL System Variable & Creating Scripts
--------------------------------------

..




SQL Related User Rights
-----------------------


Select Rights
=============

In TROIA Platform, it is possible to define some restrictions for users (or/and user profiles) to avoid unauthorized data access. This infrastructure is called "Select Rights" in TROIA jargon. Select Rights will be discussed in next sections with the other SQL rights. 





Delete Rights
=============
.


Insert Rights
=============
.


Update Rights
=============
.
