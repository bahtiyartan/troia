

==================
Database Access
==================

As a business level programming language, TROIA has high collaboration with databases. With TROIA it is possible to perform too many operations on databases, such as connecting different databases, managing database transactions or executing sql queries. This section aims to introduce database operations and persistency flags of tables.

Database Connections of Session
-------------------------------
.

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
