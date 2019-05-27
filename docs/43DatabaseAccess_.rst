

==================
Database Access
==================

As a business level programming language, TROIA has high collaboration with databases. With TROIA it is possible to perform too many operations on databases, such as connecting different databases, managing database transactions or executing sql queries. This section aims to introduce database operations and persistency flags of tables.


Database Connections
--------------------

When a user logs in the system, system firstly finds database configuration from database section of server settings file using DBServer and DBName parameters from user's login parameters. This configuration contains reqired parameters to establish database connection. Application server establishes two different database connections to same database using this configuration. Therefore; in ordinary cases, TROIA programmers do not need to make any operation to connect database to run queries, it automatically gets ready for TROIA programmers use without any TROIA level effort. It is also same for closing these connections, it is totally handled by the interpreter and closed while user logging out.

This two connections are called "first db connection" and "second db connection" in TROIA jargon and has different purposes. First database connection is totally assigned to TROIA operations. In other words if your application contains a SELECT statement, system runs select query using first database connection. All TROIA level SQL commands uses first database connection. Second database connection is assigned for interpreter level database operations like inserting logs, gathering transaction's first dialog etc. 

Actually it is not possible to run data manipulation (SELECT,UPDATE,INSERT, DELETE) and data definition (CREATE, ALTER etc.) queries using second database connection, except some special troia commands like ENQUE, DEQUE. Therefore you can just ignore second database connection as a TROIA programmer.

As you may predict, database connection is a property or feature of user's session, so even if user opens more than one transaction all transactions uses same database connection. This means database connection is shared by all open transactions, and if a session changes state or a feature of connection this state/feature is valid for other transactions. Actually this is one of the main technical reasons of why it is not possible to run more than one processes on different transactions simultaneously. For some special cases like multithreading, it is possible to establish dedicated database connections for each transaction. We will discuss this advanced issue in next sections, but for now you can just ignore dedicated database connections for transactions.

Briefly, system automatically establishes a database connection on user login to serve for all open TROIA applications and all SELECT, UPDATE, INSERT and DELETE commands uses this database connection.


Selecting data from Database
----------------------------

To run a select query on database, fetch all dataset and assign selected data to a table symbol, you must use SELECT command. SELECT command is very similar to SQL's select command and supports almost all stuff like where conditions, distict, inner/outer joins, system functions, group by and order by etc. But this does not mean that SELECT command of TROIA is identical with the SQL's select command, they have different behaviours, features and their own way for some common behaviour. One of the most important things to know about SELECT command is that TROIA SELECT command is interpreted and converted to SQL Select statements considering connected database system (oracle, mssql, mysql, postgresql etc.) on runtime to create cross database queries. 

Here is the basic syntax of SELECT command:

::

	SELECT [ALL | DISTINCT] {selectlist}
		FROM {table} [, {othertables}]
		[INNER | OUTER JOIN {jointable} ON {onclause}]
		[WHERE {condition}]
		[ORDERBY {orderbycolumns} [ASC | DESC]]
		[INTO {targettable}]
		[ROWFETCHSTART {fetchstart}]
		[ROWFETCHLIMIT {fetchstart}]
		[WITHCONTROL {rowcount}]
		[WITHCACHE]

As you can see, it is very similar to a standard SQL syntax but still there are some special differences. One of the differences is about ORDER BY keyword.In TROIA you must use ORDERBY instead of "ORDER BY". Honestly, using ORDERBY instead of standard one is just a TROIA tradition which comes from old versions of troia platform, ORDER BY is also supported by new interpreter. Even though there is no difference between them, most of troia programmers still uses the one without space character. 

INTO keyword is just for indicating the target table variable. SELECT command assigns selected data to target table. If target table is not given, SELECT command creates table variable with the name of selected database table.

ROWFETCHSTART / ROWFETCHLIMIT keywords are very similar to MySQL's LIMIT and OFFSET keywords. ROWFETCHSTART commands just a kind of shift or offset to start fetching, for example if you pass 10 as ROWFETCHSTART, first nine records are ingored and fetching starts from tenth row. ROWFETCHLIMIT limits fetched row count, but of course if there are more rows than the given number. It is possible to use these keywords together or each one alone. Important point for these two keyword is that, they are totally handled on application server layer, in other words they are not affect sql query sent to database server even if database supports a similar behaviour like mysql.

WITHCONTROL variation, if your select statement selects more rows than given row count system returns to client and asks user to perform or cancel select operation to avoid possible memory problems on server and client side.

WITHCACHE is a performance optimization parameter, we will discuss it on next sections.

Here is a sample select statement selects ten users who have maximum password validity with a hardcode where condition and assings final table to a table variable.

::

	OBJECT:
		TABLE TABLEVARIABLE,
		INTEGER ROWCOUNT;
		
	SELECT CLIENT, USERNAME, PWDVALIDITY 
		FROM IASUSERS
		WHERE CLIENT = '00'
		ORDER BY PWDVALIDITY DESC
		ROWFETCHLIMIT ROWCOUNT
		INTO TABLEVARIABLE;
	
	
SQL System Variable
===================
SQL system variable is a predefined string which is set by interpreter automatically, automatically.

		

Using Troia Variables on WHERE Condition
========================================

It is also possible to use TROIA variables in SELECT statements. Interpreter automatically binds your variables to final query considering data type.

::

	OBJECT:
		TABLE TABLEVARIABLE,
		STRING NAMEPREFIX,
		INTEGER ROWCOUNT;
		
		NAMEPREFIX = 'a%';


	SELECT CLIENT, USERNAME, PWDVALIDITY 
		FROM IASUSERS
		WHERE CLIENT = SYS_CLIENT AND USERNAME LIKE NAMEPREFIX
		ORDER BY PWDVALIDITY DESC
		ROWFETCHLIMIT ROWCOUNT
		INTO TABLEVARIABLE;


Forcing Indexes
===============

...


Complex Select Statements
=========================

	
	
Select Rights
=============
.


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



USING Non-Standart Functions
----------------------------
	concat
	left
	special date functions


EXECUTESQL Command
------------------
execute sql.


SQL System Variable & Creating Scripts
--------------------------------------

..


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


Dedicated Database Connections for Transactions
-----------------------------------------------

...


Application Performance and Database
------------------------------------

performance.

WITHCACHE ...
