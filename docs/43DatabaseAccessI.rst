

==================================
Database Basics and Selecting Data
==================================

As a business level programming language, TROIA has high collaboration with databases. With TROIA it is possible to perform too many operations on databases, such as connecting different databases, managing database transactions or executing sql queries. This section aims to introduce basics of database concept in TROIA and select statement.


Basics of Database Connection in TROIA
--------------------------------------

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
When a database query is executed, system automatically the query is set to SQL, so its possible to read final database query from SQL system variable.
This behavior is same for SELECT, DELETE, UPDATE and INSERT commands. Additionally, INSERTSQL, UPDATESQL, DELETESQL commands sets this SQL system variable without running query on database. They are mostly used for creting sql scripts.

Here is a sample TROIA code that you can read value of SQL system variable.

::

	OBJECT:
		TABLE TABLEVARIABLE,
		INTEGER ROWCOUNT,
		STRING STRINGVAR3;
		
	SELECT CLIENT, USERNAME, PWDVALIDITY 
		FROM IASUSERS
		WHERE CLIENT = '00'
		ORDER BY PWDVALIDITY DESC
		ROWFETCHLIMIT ROWCOUNT
		INTO TABLEVARIABLE;
		
	STRINGVAR3 = SQL;


SQL System variable is useful system variable that you can use while learning behaviour of TROIA database commands.

		

Using Troia Variables on WHERE Condition
========================================

It is also possible to use TROIA variables in SELECT statements. Interpreter automatically binds your variables to final query considering data type. Here is a sample TROIA code that contains troia variables in its WHERE condition. As we discussed in previous sections we can analyse final sql statement from SQL system variable.

::

	OBJECT:
		TABLE TABLEVARIABLE,
		STRING NAMEPREFIX,
		STRING STRINGVAR3;
		
		NAMEPREFIX = 'a%';


	SELECT CLIENT, USERNAME, PWDVALIDITY 
		FROM IASUSERS
		WHERE CLIENT = SYS_CLIENT AND USERNAME LIKE NAMEPREFIX
		ORDER BY PWDVALIDITY DESC
		INTO TABLEVARIABLE;
		
	STRINGVAR3 = SQL;
	
There is not any limitation related with the size, count or type of binded variables.

As default, sql statement that passed to database server is same with the content of SQL variable. As an alternative binding method system can use "prepared" option, due to database configuration line which is defined on ServerSettings file of application server. In this "prepared" option, SQL variable is builded in additon to sql statement that is passed to database.


Complex Select Statements
-------------------------

In regular SELECT statements the structure of select command is defined explicitly. In other words, in a regular SELECT statement column list, table name(s) and where condition are constants  and they can be read on development time by the programmer. The only dynamic content is variables that will be binded on runtime and this variables does not change the structure of SELECT statement.

But in some cases,  some structural parts of SELECT statement are dynamic and final select statement is defined on runtime. This kind of implicit SELECT commands are called "complex select" as a TROIA jargon. Example below, shows a complex SELECT statement that contains dynamic column list, where condition and table name. Of course its possible to use only one dynamic part.

::

	OBJECT: 
		STRING SELECTITEMSLIST,
		STRING FROMTABLENAME,
		STRING WHERECONDITION,
		STRING STRCREATEDBY,
		TABLE TMPTABLE;

	SELECTITEMSLIST = 'USERNAME, PASSW, CREATEDBY, CREATEDAT';
	FROMTABLENAME = 'IASUSERS';
	WHERECONDITION = 'CREATEDBY = STRCREATEDBY';
	STRCREATEDBY = 'btan';
	/**/
	SELECT @SELECTITEMSLIST 
		FROM @FROMTABLENAME 
		WHERE @WHERECONDITION 
		INTO TMPTABLE;
		
In this example, none of the variable names are predefined; so programmers can use any variable name to indicate column list, table name and where condition. But in some cases, while first part of select statement is defined explicitly, programmers may need add an dynamic condition to WHERE condition. In this kind of cases, TROIA programmers have to use SYSADDITIONALCRITERIA system variable which is predefined additional condition variable. Here is an example that shows how to use SYSADDITIONALCRITERIA variable.

::

	OBJECT: 
		STRING FROMTABLENAME,
		STRING STRCREATEDBY,
		TABLE TMPTABLE;

	FROMTABLENAME = 'IASUSERS';
	SYSADDITIONALCRITERIA = 'AND CREATEDBY = STRCREATEDBY';
	STRCREATEDBY = 'btan';
	/**/
	SELECT USERNAME, PASSW, CREATEDBY, CREATEDAT 
		FROM @FROMTABLENAME 
		WHERE CLIENT = SYS_CLIENT @SYSADDITIONALCRITERIA 
		INTO TMPTABLE;
		
It is not allowed to combine explicit and implicit parts together on select items list and from table name. Its just allowed for WHERE condition with SYSADDITIONALCRITERIA system variable.


Performance of Complex SELECT Statements
========================================

Like other TROIA commands SELECT statements are parsed and interpreted on convert process (for mor information please see Language Basics Section). Since converting is a development time operation, it does not consume time in runtime. When it comes to complex SELECT statements, all interpreting operation is performed on runtime, because final select statement is builded on runtime. Therefore, using complex SELECT statemens has a potential to effect application performance, although it has a limited.

Briefly, although complex SELECT eases some operations, it has a negative effect on application performance, so it is not recommended to use these kind of statements when they are unnecessary.


Fething Manually
----------------

selectline , fetch



Forcing Indexes
---------------

...


USING Non-Standart Functions
----------------------------
	concat
	left
	special date functions


EXECUTESQL Command
------------------
execute sql.


Application Performance and Database
------------------------------------

performance.

WITHCACHE ...


Select Rights
-------------
.
