

============================
Selecting Data From Database
============================

*As a business level programming language, TROIA has high collaboration with databases. With TROIA it is possible to perform too many operations on databases, such as connecting different databases, managing database transactions or executing SQL queries. This section aims to introduce basics of database concept in TROIA and select statement.*



SELECT Command
--------------

To run a select query on database, fetch all dataset and assign selected data to a table symbol, you must use SELECT command. SELECT command is very similar to SQL's select command and supports almost all stuff like where conditions, distinct, inner/outer joins, system functions, group by and order by etc. But this does not mean that SELECT command of TROIA is identical with the SQL's select command, they have different behaviors, features and their own way for some common behavior. One of the most important things to know about SELECT command is that TROIA SELECT command is interpreted and converted to SQL Select statements considering connected database system (Oracle, MsQql, MySql, PostgreSql etc.) on runtime to create cross database queries. 

Here is the basic syntax of SELECT command:

::

	SELECT [ALL | DISTINCT] {selectlist}
		FROM {table} [, {othertables}]
		[INNER | OUTER JOIN {jointable} ON {onclause}]
		[WHERE {condition}]
		[ORDERBY {orderbycolumns} [ASC | DESC]]
		[INTO {targettable}]
		[ROWFETCHSTART {fetchstart}]
		[ROWFETCHLIMIT {fetchlimit}]
		[WITHCONTROL {rowcount}]
		[WITHCACHE]

As you can see, it is very similar to a standard SQL syntax but still there are some special differences. One of the differences is about ORDER BY keyword. In TROIA you must use ORDERBY instead of "ORDER BY". Honestly, using ORDERBY instead of standard one is just a TROIA tradition which comes from old versions of troia platform, ORDER BY is also supported by new interpreter. Even though there is no difference between them, most of troia programmers still uses the one without space character. 

INTO keyword is just for indicating the target table variable. SELECT command assigns selected data to target table. If target table is not given, SELECT command creates table variable with the name of selected database table.

ROWFETCHSTART / ROWFETCHLIMIT keywords are very similar to MySQL's LIMIT and OFFSET keywords. ROWFETCHSTART commands just a kind of shift or offset to start fetching, for example if you pass 10 as ROWFETCHSTART, first nine records are ignored and fetching starts from tenth row. ROWFETCHLIMIT limits fetched row count, but of course if there are more rows than the given number. It is possible to use these keywords together or each one alone. Important point for these two keyword is that, they are totally handled on application server layer, in other words they are not affect SQL query sent to database server even if database supports a similar behavior like MySql.

WITHCONTROL variation, if your select statement selects more rows than given row count system returns to client and asks user to perform or cancel select operation to avoid possible memory problems on server and client side.

WITHCACHE is a performance optimization parameter, we will discuss it on next sections.

Here is a sample select statement selects ten users who have maximum password validity with a hardcode where condition and assigns final table to a table variable.

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
When a database query is executed, system automatically the query is set to SQL, so it is possible to read final database query from SQL system variable.
This behavior is same for SELECT, DELETE, UPDATE and INSERT commands. Additionally, INSERTSQL, UPDATESQL, DELETESQL commands sets this SQL system variable without running query on database. They are mostly used for creating SQL scripts.

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


SQL System variable is useful system variable that you can use while learning behavior of TROIA database commands.

		

Using Troia Variables on WHERE Condition
========================================

It is also possible to use TROIA variables in SELECT statements. Interpreter automatically binds your variables to final query considering data type. Here is a sample TROIA code that contains troia variables in it's WHERE condition. As we discussed in previous sections we can analyze final SQL statement from SQL system variable.

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
	
There is not any limitation related with the size, count or type of bound variables.

As default, SQL statement that passed to database server is same with the content of SQL variable. As an alternative binding method system can use "prepared" option, due to database configuration line which is defined on server configuration (server settings) file of application server. In this "prepared" option, SQL variable is builded in addition to SQL statement that is passed to database.


Complex Select Statements
-------------------------

In regular SELECT statements the structure of select command is defined explicitly. In other words, in a regular SELECT statement column list, table name(s) and where condition are constants and they can be read on development time by the programmer. The only dynamic content is variables that will be bound on runtime and this variables does not change the structure of SELECT statement.

But in some cases, some structural parts of SELECT statement are dynamic and final select statement is defined on runtime. This kind of implicit SELECT commands are called "complex select" as a TROIA jargon. Example below, shows a complex SELECT statement that contains dynamic column list, where condition and table name. Of course it is possible to use only one dynamic part.

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
		
It is not allowed to combine explicit and implicit parts together on select items list and from table name. It is just allowed for WHERE condition with SYSADDITIONALCRITERIA system variable.


Performance of Complex SELECT Statements
========================================

Like other TROIA commands SELECT statements are parsed and interpreted on convert process (for mor information please see Language Basics Section). Since converting is a development time operation, it does not consume time in runtime. When it comes to complex SELECT statements, all interpreting operation is performed on runtime, because final select statement is builded on runtime. Therefore, using complex SELECT statements has a potential to effect application performance, although it has a limited.

Briefly, although complex SELECT eases some operations, it has a negative effect on application performance, so it is not recommended to use these kind of statements when they are unnecessary.


Fetching Manually
----------------

SELECT command performs all steps of select operation in an atomic way. But in some cases selected data is huge and fetching all data may consume too much memory. Assume that you will perform a specific operation for each row of a huge table and you don't need to fetch and store this huge data in memory. In this case you must run select query in database and fetch data row by row. To perform this kind of operation you must use SELECTLINE and FETCH commands instead of SELECT command. On the contrary of SELECT command, SELECTLINE command does not fetches selected data and waits for the FETCH command to fetch rows. As you can predict, FETCH command fetches a single row from result set to table on each execution.

Syntax of SELECTLINE statement is totally same except the name, so it is possible to use complex SELECTLINE statements similar to SELECT command. At the example below, while looping on whole result set, table contains only single row at each iteration. Please write a similar code using SELECT and LOOP command and discuss the value of STRINGVAR3.

::

	OBJECT: 
		STRING STRINGVAR3,
		TABLE TMPTABLE;

	STRINGVAR3 = '';
	/**/
	SELECTLINE USERNAME, PASSW, CREATEDBY 
		FROM IASUSERS 
		WHERE CLIENT = SYS_CLIENT AND CREATEDBY = 'btan' INTO TMPTABLE;

	WHILE 1 
	BEGIN
		FETCH TMPTABLE;
		IF SYS_STATUS THEN
		   BREAK;
		ENDIF;
		/** do something with the row */
		STRINGVAR3 = STRINGVAR3 + TMPTABLE_ROWCOUNT + ' ' + TMPTABLE_USERNAME + TOCHAR(10);
	ENDWHILE;

	SET TMPTABLE TO TABLE TMPTABLE;
	
	
If you want to perform the operation block by block on the rows of the selected result set, fetch command has also supports fetching in blocks with the given row size. This feature is only supported on 8.02.01 090301 and following versions. Here is the whole available syntax options of FETCH command.

::

	FETCH {tablename}
	FETCH {tablename} SIZE {rowcounttofetch}


Database Specific Syntax & Functions
------------------------------------

As mentioned on previous titles, TROIA SQL commands are not identical to SQL commands. They are just TROIA commands that are interpreted to sql statements considering database system that user is connected. In other words, if user is connected to X database system (MySQL, Oracle, MSSQL etc.), system converts SELECT command to a valid select statement compatible with X.

Lets discuss it with an example. CONCAT function which allows string concatenation on database layer and it is supported on MySQL, but in MSSQL + operator and in Oracle || operators concatenate string variables. In TROIA applications developers have to use CONCAT() function but system converts it to + operator on MSSQL connections and || on Oracle connections. So there is no need to make manipulations on TROIA code to support multiple database systems. The list below contains some special function names that is manipulated by TROIA interpreter to support different database systems. This list is does not contains all special function names, for more information and up to date list please see help related help documents.

::

	CONCAT()            DATEPART()
	SUBSTRING()         DATEDIFF()
	LEFT()              YEAR()
	MONTH()             DAYOFMONTH()
	QUARTER()           DAYOFYEAR()
	MINUTE()            HOUR()
	DATEADD()           WEEK()
	LEN()               DATESUB()
	

Here is a more concrete example that shows how interpreter manipulates special functions considering database system. The table shows the value of STRINGVAR3 variable which contains the final SQL query for different database systems as a result of SELECT statement in the code. 

::

	OBJECT:
		TABLE T1,
		STRING STRINGVAR3;
		
	SELECT YEAR(CREATEDAT) AS YEARCOLUMN
		FROM IASUSERS
		INTO T1;
		
	STRINGVAR3 = SQL;
	
+------------+------------------------------------------------------------------------------------+
| MSSQL      |  SELECT YEAR(CREATEDAT) AS YEARCOLUMN FROM IASUSERS                                |
+------------+------------------------------------------------------------------------------------+
| PostgreSQL |  SELECT CAST(EXTRACT(YEAR FROM CREATEDAT) AS INTEGER) AS YEARCOLUMN FROM IASUSERS  |
+------------+------------------------------------------------------------------------------------+
| MySQL      |  SELECT YEAR(CREATEDAT) AS YEARCOLUMN FROM IASUSERS                                |
+------------+------------------------------------------------------------------------------------+


Besides incompatibility cases on functions on different database systems, there are various types of differences. Another example is behavior of 'IS' and '=' operators of Oracle for the NULL and empty string values. Since such incompatibility cases are handled by troia interpreter, implementing different codes for a specific database system is not recommended because of possible performance and code transfer problems.


Forcing Indexes
===============

Although it is not recommended to force database engine to use a specific index from TROIA commands, it is technically possible to indicate an index name on SELECT and SELECTLINE commands. Here is an example to force an index:

::

	SELECT * FROM IASUSERS (INDEX=indexname) WHERE CLIENT = SYSCLIENT AND ...


TROIA interpreter, automatically converts this syntax to the syntax that connected database allows. For example, in MySQL database connections it is converted to "FORCE INDEX" syntax.



Application Performance and Database Operations
-----------------------------------------------

Database operations have huge affect on application performance in TROIA, like other similar programming languages. Database related performance bottlenecks may be in different layers such as database configuration, table indexing (wrong indexing) or the query (ineffective query). 


Query level performance problems may be related with the structure of your select statement. To avoid these kind of problems you must be sure that you wrote the simplest and most effective select statement as much as possible considering requirements of your process. Even if you have the best SELECT statement to access a useless or already existing data, it will consume your cpu time. Therefore, in some cases you have to criticize even the existence of your SELECT statements, not only structure of them.


WITHCACHE Option (Database Selection Cache)
===========================================

In some cases, TROIA programmers passes same SELECT statements more than once even the expected result is same. To solve this kind of performance bottlenecks **in short term**, WITHCONTROL variation is an option. If you use WITHCACHE option, the result of SELECT statement is cached by the interpreter and same result is used when SELECT statement is performed again. 

In the example below, both SELECT statements of first block access database and executes corresponding select query on database, but in second block, first select is executed on database layer and the result is cached, then second SELECT statement uses the cached result without accessing database.


::

	/* Block 1 */
	SELECT * FROM IASUESERS WHERE CLIENT = SYS_CLIENT;
	SELECT * FROM IASUESERS WHERE CLIENT = SYS_CLIENT;
	
	/* Block 2 */
	SELECT * FROM IASUESERS WHERE CLIENT = SYS_CLIENT AND USERNAME = SYS_USER WITHCACHE;
	SELECT * FROM IASUESERS WHERE CLIENT = SYS_CLIENT AND USERNAME = SYS_USER WITHCACHE;


This kind of database access attempts are mostly used on loop statements of item tables to get data related with the head table. But the actual head record is same for all items and the select query result is same as well. **The best practice about this kind of situations is not selecting head data, but in some cases WITHCACHE option allows programmers to refactor existing code easily. It is not recommended to use WITHCACHE option if there is an alternative solution.**

Database selection caches which are created by the first SELECT statement that has WITHCACHE option, is valid for only one session and during only one user interaction. In other words, if a session caches data for a select statement, this cache cannot be used by other sessions even if they use same database connection for the same query. And this caches are cleared when action ends system returns to client side for a new user interaction like clicking button etc. Additionally, CLEARDBSELECTIONCACHE() TROIA function removes all cached data, if TROIA programmers need to clear cache.


As default, database selection cache is enabled, but it is possible to disable this option with **EnableDBSelectionCache** on server configuration file (server settings file). When database selection cache disabled, TROIA interpreter ignores WITHCACHE parameter and performs all queries on database.


