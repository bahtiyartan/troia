

===============
Basics of Table
===============

*As discussed in previous sections, table is most important data types of TROIA, because of the its extended features. This section aims to introduce basic concepts about tables and most used table command and functions.*

Introduction
------------

Table is a built-in data type of TROIA like string, integer or double. In addition to being a simple data type, table has too many features that allow programmers to implement database applications with easily. 

Thera are too many structures and concepts which are based on table data type, even charts, diagrams etc. Also database queries and interactions are based on tables. Before discussing tables and its features we must focus what we mean with the word : "table".


Database Table/Data Type/Table Control
======================================
TROIA Platform, uses database tables just for storing data, so when we discuss tables in this section we don't mean database tables. Defining database tables and using data manipulation commands will be discussed in next sections.

In TROIA, there are two main table concepts, first one is table data type and second one is "table control" that is used on dialogs. Table control has a user interface and some functions on its user interface such as conditional formatting etc. Table controls have also a table typed variable in server memory, in other words "table control" is user interface object and table is a kind of value (like textfield's background color & its value). So its possible to apply all features and operations discussed in this section, to value of a table control.

In this section we use the word table "table" to mean a variable whose type is table. Features related with table controls will be discussed in next sections.


Defining/Filling Tables
-----------------------

Like other data types, data definition commands are used to define a table and it is not so much different defining an integer variable. Here is a single object command that defines an integer and a table.

::

	OBJECT:
		TABLE TABLEVAR,
		INTEGER INTVAR;

Unlike primitive data types (int, double etc), table is a two dimensional data type. Therefore, only variable definition is not so much useful for tables.In addition to defining table instance, also we must define its structure/columns to store data. To add columns, there is a command which gets column name, column type and colcumn length and adds a column to table variable. This command is called "APPEND COLUMN", here is its syntax and sample usage:

::

	APPEND COLUMN {columnname}, {columntype}, {columnlength} TO {table};

	/* add a 100 char length string column named COL1 to TABLEVAR */
	APPEND COLUMN COL1, STRING, 100 TO TABLEVAR;
	APPEND COLUMN COL2, INTEGER, 10 TO TABLEVAR;
	
	/* now table is able to store data */
		
It is possible to add STRING, INTEGER,TEXT,DATE,DATETIME,DECIMAL,LONG, TIME and TIMES columns using APPEND COLUMN command and it is not allowed to add TABLE or VECTOR types as table column. Column names must be unique, so it is not allowed to add two columns that has same name. For more details about the command please see help documents. 

Another option to define table is using a table control on a dialog. Every ui table has a global table variable as its data model and programmers can access table data over this model. Model table has same name with table control and its not too much different from a textfield and its value (as a string variable). APPEND COLUMN also works for ui tables, but appending and showing new columns on user interface will be discussed in next section.

Additionally, SELECT command defines a table variable with its columns, so programmers do not need to define table and its columns before the command. SELECT command also adds rows depending to query result. SELECT command will be discussed in related sections, but for now we must know that, SELECT command changes the column model of an existing table variable or defines table with its column model. Here is a sample code:

::

	OBJECT:
		TABLE T1;
	
	/* change column-model and data of an existing variable */
	SELECT USERNAME, CREATEDBY, CREATEDAT FROM IASUSERS WHERE 1 = 2 INTO T1;
	
	/* define a table variable with its column-model */
	SELECT USERNAME, CREATEDBY, CREATEDAT FROM IASUSERS WHERE 1 = 2 INTO T2;


Adding Rows To Tables
---------------------

As its obvious, rows are the data of a table variable. In TROIA there are multiple ways of adding rows to a table. To add a new row to a table variable programmatically, APPEND ROW command is used.This command is able to add a new row to first row, end of the table or after a specific row. Here is the syntax and example, for more detail please see TROIA Help.

::

	APPEND ROW TO {table} [ATHEAD | ATBETWEEN];
	
	OBJECT:
		TABLE T1;

	SELECT USERNAME, CREATEDBY, CREATEDAT FROM IASUSERS WHERE 1 = 2 INTO T1;
	APPEND ROW TO T1;
	
Also it is possible to fill table with data which is selected from database. Another feature of SELECT command is adding new rows to table. Here is an example:

::

	OBJECT:
		TABLE T1;

	SELECT USERNAME, CREATEDBY, CREATEDAT FROM IASUSERS INTO T1;

	
Also it is possible to add rows to tables from user interface by users. To add a new row to a ui table, INSERT key is used. It will be discussed detailly in next sections.
		
Accessing Table Data
--------------------

To access data on a table cell, "_" operator is used. This operator needs at least two information table name & column name or an additional row number.

Using Row Index
===============

Like arrays on other programming languages, row index can be specified on cell value access. In TROIA row indexes starts from 1, here is the example:

::

	OBJECT:
		TABLE T1,
		INTEGER ROWINDEX,
		STRING RESULT;
		
	ROWINDEX = 1;
	RESULT = '';

	SELECT USERNAME, CREATEDBY, CREATEDAT FROM IASUSERS INTO T1;
	
	WHILE ROWINDEX < T1_ROWCOUNT
	BEGIN
		RESULT = RESULT + T1[ROWINDEX]_USERNAME + ' created by ';
		RESULT = RESULT + T1[ROWINDEX]_CREATEDBY + ' at ';
		RESULT = RESULT + T1[ROWINDEX]_CREATEDAT + TOCHAR(10);
		ROWINDEX = ROWINDEX + 1;
	ENDWHILE;

Accessing table cells using row indexes is not mostly used method, because in this method programmer must provice row index at each cell access.

Active Row: Internal Row Cursor
===============================

To reduce development efford, table variable have an internal cursor that shows active row,so programmers need to point row number for each cell access on same row.Here is the TROIA code that prints same information, but using active row instead of row index:

::

	OBJECT: 
		TABLE T1,
		INTEGER ROWINDEX,
		STRING RESULT;

	ROWINDEX = 1;
	RESULT = '';
	SELECT USERNAME, CREATEDBY, CREATEDAT 
		FROM IASUSERS 
		INTO T1;

	WHILE ROWINDEX < T1_ROWCOUNT 
	BEGIN
		T1_ACTIVEROW = ROWINDEX;
		RESULT = RESULT + T1_USERNAME + ' created by ';
		RESULT = RESULT + T1_CREATEDBY + ' at ';
		RESULT = RESULT + T1_CREATEDAT + TOCHAR(10);
		ROWINDEX = ROWINDEX + 1;
	ENDWHILE;

Active row is the most used and important concept of working with tables. Most of table commands are based on active row, such as looping or locating. 

"Active Row" is not same with "Selected Row", it always points a single row. This curser value is set by user interface or different TROIA commands. We will discuss commands which effects active row in related sections. But in this section we must know that it is possible to set active row directly, by two main method.

First one is directly setting the value with READ command's INDEX variation, here is the basic syntax of read command:

::

	/* to a given index */
	READ {table} WITH INDEX {activerowindex};
	
	/* to an index related with current */
	READ {table} WITH FIRST | LAST | NEXT | PREV;	
	
Another method is setting active row value with ACTIVEROW flag of table. 


Table Flags
-----------

There are some special fields that returns specific information about tables like row count, active row etc. Names of these fields are reserved and can not be used as column names. And can be accessed by _ operator like table columns.


Flags about Table Information
=============================

Here are flags that returns data about table's itself (independent from active row). Some of this flags will be discussed (w.b.d.) detailly, in related sections.

+--------------+-------+------+-------------------------------------------------+
|Flag          |Type   |R-Only| Description                                     |
+--------------+-------+------+-------------------------------------------------+
|ACTIVEROW     |INTEGER| NO   | Returns active row index between 1-row count    |
+--------------+-------+------+-------------------------------------------------+
|ROWCOUNT      |INTEGER| YES  | Returns number of rows in table                 |
+--------------+-------+------+-------------------------------------------------+
|DBTABLENAME   |STRING | YES  | Db table whose data is set to table (w.b.d.)    |
+--------------+-------+------+-------------------------------------------------+
|HASSELECTEDROW|INTEGER| YES  | Returns whether given table has a selected row  |
+--------------+-------+------+-------------------------------------------------+
|ACTIVECOL     |INTEGER| YES  | Active column index of ui table                 |
+--------------+-------+------+-------------------------------------------------+
|ACTIVECOLNAME |STRING | YES  | Active column name of ui table (w.b.d.)         |
+--------------+-------+------+-------------------------------------------------+
|ARROWSTATE    |INTEGER| YES  | For the ArrowClick Event of ui table (w.b.d.)   |
+--------------+-------+------+-------------------------------------------------+


Flags about UI Table Rows
=========================

Additionally, table has row releated flags, each row is able to store different values for each flag. This flags also accessed by with and without row index. If row index is not specified system returns active row's flag value. Although this flags are related/meaningful for ui tables, they are also available for any table variable. But it is not recommended to use this flags for other purposes except their main purpose.

+----------+-------+----------------------------------------------------------------------+
|Flag      |Type   | Description                                                          |
+----------+-------+----------------------------------------------------------------------+
|SELECTED  |INTEGER| is set to 1 when user selects row on ui, otherwise 0                 |
+----------+-------+----------------------------------------------------------------------+
|HIDE      |INTEGER| if it is set to 1, row is not visible on ui table                    |
+----------+-------+----------------------------------------------------------------------+
|BKCOLOR   |INTEGER| to change row color on ui                                            |
+----------+-------+----------------------------------------------------------------------+
|ROWTOOLTIP|STRING | tooltip text which gets visible when user mouse stops on row         |
+----------+-------+----------------------------------------------------------------------+
|FYI       |STRING | another alias for ROWTOOLTIP                                         |
+----------+-------+----------------------------------------------------------------------+
|FILTERED  |INTEGER| is set to 1 when row is invisible by a ui filter                     |
+----------+-------+----------------------------------------------------------------------+
|SUMMARYROW|INTEGER| must be set to 1, for subtotal/summary rows to avoid miscalculations |
+----------+-------+----------------------------------------------------------------------+

There are some other ui table flags, they will be discussed in related sections.
	
Persistency Flags
=================

TROIA tables also support object/relational persistency, so programmers don't need to store extra information to select appropriate data manimulation statement for storing data on database. Persistency state which indicates row's state compared to releated record on database, is stored on persistency flags. They are set automatically when a cell value changed or when data is read from database by interpreter. Here is a short and simple list of persistency flags, each flag and its state transitions will be discussed detailly on database section.

+----------+----------------------------------------------------+
|Flag      | Description                                        |
+----------+----------------------------------------------------+
|DELETED   | Returns whether row is deleted by user, programmer |
+----------+----------------------------------------------------+
|INSERTED  | Returns whether row is a new row.                  |
+----------+----------------------------------------------------+
|READ      | Returns whether row read from database.            |
+----------+----------------------------------------------------+
|UPDATED   | Returns whether row is updated after db read.      |
+----------+----------------------------------------------------+
	
Looping on Tables
-----------------

In TROIA, to do something for each row of a table, there is no need to define an index and loop with a while statement and increase index at each iteration. Other option is LOOP command which is very similer to for-each statements on some other programming languages. Also it supports to add some conditions so programmers don't need to add an if statement to loop block. Here is the simplest syntax:

::

	LOOP AT table [WHERE {condition}]
	BEGIN
		block
	ENDLOOP;
	
As it is obvious LOOP command, increases active row and executes condition new active row and if condition is returns true runs its inner block, so we must know that active row is not same at before the loop and after the loop. Here is the same example that uses LOOP command instead of WHILE.

::

	OBJECT: 
		TABLE T1,
		STRING RESULT;

	RESULT = '';
	SELECT USERNAME, CREATEDBY, CREATEDAT 
		FROM IASUSERS 
		INTO T1;

	LOOP AT T1 
	BEGIN
		RESULT = RESULT + T1_USERNAME + ' created by ';
		RESULT = RESULT + T1_CREATEDBY + ' at ';
		RESULT = RESULT + T1_CREATEDAT + TOCHAR(10);
	ENDLOOP;
	
And another example that uses where condition. In this example, it prints users only created by 'BTAN'.

::

	OBJECT: 
		TABLE T1,
		STRING RESULT;

	RESULT = '';
	SELECT USERNAME, CREATEDBY, CREATEDAT 
		FROM IASUSERS 
		INTO T1;
	
	LOOP AT T1 WHERE T1_CREATEDBY == 'admin'
	BEGIN
		RESULT = RESULT + T1_USERNAME + ' created by ';
		RESULT = RESULT + T1_CREATEDBY + ' at ';
		RESULT = RESULT + T1_CREATEDAT + TOCHAR(10);
	ENDLOOP;
	
Both with and without WHERE condition this method scans whole table. If where condition provided LOOP command executes condition expression for each row. 


Looping Faster: CRITERIA COLUMNS Variation
==========================================

Another option without a where condition expression is using CRITERIA COLUMNS variation of LOOP Command. In this variation system does not execute condition expression on TROIA interpereter layer, but in java layer. This variation works faster than scanning whole table on TROIA layer


::
	
	LOOP AT {table} CRITERIA COLUMNS {columns} VALUES {values} [NOTCASESENSITIVE]
	BEGIN
		block
	ENDLOOP
	
	/* {columns} & {values} are comma separated list. */
	
Here is an example, that shows looping with CRITERIA COLUMNS. This examle prints users that is created by BTAN and password validity is 2000:

::

	OBJECT: 
		TABLE T1,
		STRING RESULT,
		STRING STRCREATEDBY;

	SELECT USERNAME, CREATEDBY, PWDVALIDITY 
		FROM IASUSERS 
		INTO T1;
		
	STRCREATEDBY = 'BTAN';
	RESULT = '';

	LOOP AT T1 CRITERIA COLUMNS CREATEDBY,PWDVALIDITY VALUES STRCREATEDBY,2000 
	BEGIN
		RESULT  = RESULT + T1_USERNAME + ':';
		RESULT = RESULT  + T1_PWDVALIDITY + TOCHAR(10);
	ENDLOOP;
	
	
Please change value of STRCREATEDBY and test with both case sensitive and insensitive options, with single and multiple parameters.


Looping Faster: Loop on Hash Index
==================================

Also it is possible to create virtual indexes on a table variable to loop on different values of indexed columns. While creating an index, system finds/calculates indexes of rows for each value combination of indexed rows. So if you need multiple loops and different criteria values for same columns, calculating indexes for once may be useful for high performance looping.

To create virtual hashes on a table BUILDHASHINDEX command is used, here is the syntax:

::

	BUILDHASHINDEX {indexname} COLUMNS {columns} ON {table} [FORCE];
	
BUILDHASHINDEX command does not recreate index if there is already an index with given index name. If you want to overwrite an existing index you must use optional FORCE parameter. Also HASHASHINDEX() function is useful to check whether an index exists with same name. For more information about hash indexes and related functions please see help documents. Here is a simple example for looping over hash indexes.

::

	OBJECT: 
		TABLE T1,
		STRING RESULT,
		STRING INDEXNAME;

	SELECT USERNAME, CREATEDBY,PWDVALIDITY 
		FROM IASUSERS 
		INTO T1;

	INDEXNAME = 'myindex';
	RESULT = '';
	BUILDHASHINDEX INDEXNAME COLUMNS PWDVALIDITY,CREATEDBY ON T1;

	LOOP AT T1 CRITERIA INDEXED INDEX INDEXNAME VALUES 2000, 'BTAN' 
	BEGIN
		RESULT  = RESULT + T1_USERNAME + ':';
		RESULT = RESULT  + T1_PWDVALIDITY + TOCHAR(10);
	ENDLOOP;

	RESULT = RESULT + '----------------' + TOCHAR(10);

	LOOP AT T1 CRITERIA INDEXED INDEX INDEXNAME VALUES 60, 'kkizir' 
	BEGIN
		RESULT  = RESULT + T1_USERNAME + ':';
		RESULT = RESULT  + T1_PWDVALIDITY + TOCHAR(10);
	ENDLOOP;
	

Which Looping Method is the Best?
---------------------------------

As mentioned in previous sections, tables are the main and most used data type of TROIA programming language. Tables store bulk data because of its nature, so considering performance issues while working with tables is a must. Looping is also very important process for high performance applications, so programmers must select correct looping option considering their data structure, search criterias. 

As an important point, programmers must use LOOP command to loop on tables. In other words using WHILE command is NOT recommended. Also it is very important to select correct LOOP command option considering data structure of table, search criterias, expressions on criterias, number of loops etc. 

Each looping option has different pros & cons. The main rule that programmer must consider is "using java layer as much as possible instead of TROIA commands". For example, if it is possible using CRITERIA columns is faster than WHERE condition.

The second rule is "calculating same expressions once". If expression result is not related cell of active row, calculating comparison values before the loop will be useful. Here is an example:

::

	OBJECT: 
		TABLE T1,
		STRING RESULT;

	RESULT = '';
	SELECT USERNAME, PWDVALIDITY 
		FROM IASUSERS 
		INTO T1;
	
	/* calculate value for each row */
	LOOP AT T1 WHERE T1_PWDVALIDITY == THIS.CALCMAXVALIDITY() * 4 + 40
	BEGIN
		RESULT = RESULT + T1_USERNAME + '-> max validity ' + TOCHAR(10);
	ENDLOOP;
	
Here is faster alternative which calculate value before looping:
	
::

	OBJECT: 
		TABLE T1,
		STRING RESULT,
		INTEGER MAXVALIDITY;

	RESULT = '';
	SELECT USERNAME, PWDVALIDITY 
		FROM IASUSERS 
		INTO T1;
	
	MAXVALIDITY = THIS.CALCMAXVALIDITY() * 4 + 40;

	LOOP AT T1 WHERE T1_PWDVALIDITY == MAXVALIDITY
	BEGIN
		RESULT = RESULT + T1_USERNAME + '-> max validity ' + TOCHAR(10);
	ENDLOOP;
	

Here is faster alternative which moves comparison expression to java layer with CRITERIA COLUMNS:

::

	OBJECT: 
		TABLE T1,
		STRING RESULT,
		INTEGER MAXVALIDITY;

	RESULT = '';
	SELECT USERNAME, PWDVALIDITY 
		FROM IASUSERS 
		INTO T1;
	
	MAXVALIDITY = THIS.CALCMAXVALIDITY() * 4 + 40;

	LOOP AT T1 CRITERIA COLUMNS PWDVALIDITY VALUES MAXVALIDITY
	BEGIN
		RESULT = RESULT + T1_USERNAME + '-> max validity ' + TOCHAR(10);
	ENDLOOP;
	
	
Locating on Table
-----------------

In some cases, programmers loops on table to find a specific row and do something for this row only. In TROIA, programmers do not need to loop to find a row, thanks to LOCATERECORD command. This command finds the correct row due to given parameters and changes the active row cursor on table. If LOCATERECORD command can not find correct row with given parameters it does not changes the active row and sets SYS_STATUS to 1, so programmer use check whether there is a row with given parameters. Here is the basic and most used variation of LOCATERECORD command:

::
	
	LOCATERECORD SEQUENTIAL COLUMNS {columns} VALUES {values} 
	                                ON {table} [NOTCASESENSITIVE] [NEXT] [LAST];
	
An example that prints two users which is created by 'BTAN', please change the value and test/run with different parameters and variations.

::

	OBJECT:
		TABLE T1;

	SELECT USERNAME, CREATEDBY,CHANGEDBY 
		FROM IASUSERS 
		INTO T1;

	SET TMPTABLE TO TABLE TMPTABLE;

	LOCATERECORD SEQUENTIAL COLUMNS CREATEDBY VALUES 'BTAN' ON T1;
	STRINGVAR3 = T1_ACTIVEROW  + '-'+ T1_USERNAME;
	STRINGVAR3 = STRINGVAR3 + ' ('+ SYS_STATUS + ')';

	LOCATERECORD SEQUENTIAL COLUMNS CREATEDBY VALUES 'BTAN' ON T1 NEXT;
	STRINGVAR3 = STRINGVAR3 + TOCHAR(10) +T1_ACTIVEROW  + '-'+ T1_USERNAME;
	STRINGVAR3 = STRINGVAR3 + ' ('+ SYS_STATUS + ')';
	
SEQUENTIAL variation of LOCATERECORD command is very similar to CRITERIA COLUMNS variation of LOOP command and scans table on java layer. As an alternative to scanning table, command has an BINARYSEARCH variation which searches faster on as sorted table. BINARYSEARCH variation only works on sorted tables on given columns (ascending). Here is the syntax and a simple column example:

::

	LOCATERECORD BINARYSEARCH COLUMNS {columns} VALUES {values} 
	                                  ON {table} [NOTCASESENSITIVE];

::

	OBJECT:
		TABLE T1;

	SELECT USERNAME, CREATEDBY,CHANGEDBY 
		FROM IASUSERS
		ORDERBY CREATEDBY 
		INTO T1;

	LOCATERECORD BINARYSEARCH COLUMNS CREATEDBY VALUES 'BTAN' ON T1;
	STRINGVAR3 = T1_ACTIVEROW  + '-'+ T1_USERNAME;
	STRINGVAR3 = STRINGVAR3 + ' ('+ SYS_STATUS + ')';


Also it is possible to locate table's active row over an hashindex. With this option, programmers can create index once and locate rows with different row values using this index. This variations reduces to process time compared to scanning table on each locate. Here is the syntax and example:

::
	
	LOCATERECORD INDEXED INDEX {indexname} VALUES {values} 
	                                       ON {table} [NEXT] [LAST];

::

	OBJECT: 
		TABLE T1;

	SELECT USERNAME, CREATEDBY,CHANGEDBY 
		FROM IASUSERS
		INTO T1;
		
	BUILDHASHINDEX 'MyIndex' COLUMNS CREATEDBY ON T1;

	LOCATERECORD INDEXED INDEX 'MyIndex' VALUES 'BTAN' ON T1;
	STRINGVAR3 = T1_ACTIVEROW  + '-'+ T1_USERNAME;
	STRINGVAR3 = STRINGVAR3 + ' ('+ SYS_STATUS + ')';
	
	LOCATERECORD INDEXED INDEX 'MyIndex' VALUES 'BTAN' ON T1 LAST;
	STRINGVAR3 = STRINGVAR3 + TOCHAR(10) + T1_ACTIVEROW  + '-'+ T1_USERNAME;
	STRINGVAR3 = STRINGVAR3 + ' ('+ SYS_STATUS + ')';

	
As it is obvious, LOCATERECORD variations are similar to LOOP command's. Each variation has different pros and cons. To create high performance applications, programmers must decide correct variation due to data structure, locate count etc. For more details about locating on tables please see help documents of LOCATERECORD command.
	
Sorting Table Data
------------------

It is possible to select sorted row data from database but in some cases programmers may need sorting tables on application layer. To sort table variables due to given columns, SORT command is used. Its possible to sort rows descending and ascending order due to one or more columns. Default sorting is ascending and not case sensitive. Basic syntax is below:

::

	SORT {tablename} [CASESENSITIVE] ON [DESC] {column1}, [DESC] {column2};

Here is a simple example that sorts users due to who defined them and definition time.

::

	OBJECT:
		TABLE TMPTABLE;

	SELECT USERNAME, CREATEDBY, CREATEDAT
	FROM IASUSERS
	INTO TMPTABLE;

	SORT TMPTABLE ON CREATEDBY, DESC CREATEDAT;

	/* this command will be discussed later */
	SET TMPTABLE TO TABLE TMPTABLE;

Also its possible to provide sort colums dynamically, with the syntax below:

::

	SORT {tablename} [CASESENSITIVE] ON @{columnsastext};

This syntax allows programmers to sort dynamically, without runtime interpretation *(runtime interpretation will be discussed later)*. Here is the same example that uses dynamic syntax:

::

	OBJECT:
		TABLE TMPTABLE,
		STRING STRINGVAR3;

	SELECT USERNAME, CREATEDBY, CREATEDAT
	FROM IASUSERS
	INTO TMPTABLE;

	STRINGVAR3 = 'CREATEDBY, DESC CREATEDAT';

	SORT TMPTABLE ON @STRINGVAR3;

	/* this command will be discussed later */
	SET TMPTABLE TO TABLE TMPTABLE;


Sort in Hierarchical Order
==========================	

In some cases, programmers may need to sort rows in a relational manner. If table has an id column and parent id column, TROIA has an advanced sorting option such tables. Here is a sample data mode that stores two countries and cities.

+------------+-----------------+
|**COUNTRY** | **NAME**        |
+------------+-----------------+
|GERMANY     | KARLSRUHE       |
+------------+-----------------+
|GERMANY     | BERLIN          |
+------------+-----------------+
|TURKEY      | IZMIR           |
+------------+-----------------+
|TURKEY      | ISTANBUL        |
+------------+-----------------+
|            | TURKEY          |
+------------+-----------------+
|            | GERMANY         |
+------------+-----------------+

And you need data model after hierarchical sort:

+------------+-----------------+
|**COUNTRY** | **NAME**        |
+------------+-----------------+
|            | GERMANY         |
+------------+-----------------+
|GERMANY     | KARLSRUHE       |
+------------+-----------------+
|GERMANY     | BERLIN          |
+------------+-----------------+
|            | TURKEY          |
+------------+-----------------+
|TURKEY      | IZMIR           |
+------------+-----------------+
|TURKEY      | ISTANBUL        |
+------------+-----------------+

To sort tables in hierarchical manner to get tree like table, SORT HIERARCHICAL variation is used. Here is the syntax:

::

	SORT {table} HIERARCHICAL IDCOLUMN {idcolumn} PARENTIDCOLUMN {parentidcolumn}
                                   [ROOTINDICATOR {indicatorvalue}] [MARKLEAFSASNODE];
  
Values on id columns must be unique. And both for id and parentid columns must be single row, if data has multiple colums for id and parentid they must be consolidated before SORT HIERARCHICAL command. Here is sample TROIA code that creates and sorts sample data model:

::

	SELECT USERNAME AS COUNTRY, CREATEDBY AS NAME
		FROM IASUSERS
		WHERE 1=2
		INTO TMPTABLE;

	APPEND ROW TO TMPTABLE;
	TMPTABLE_COUNTRY = 'TURKEY';
	TMPTABLE_NAME = 'IZMIR';

	APPEND ROW TO TMPTABLE;
	TMPTABLE_NAME = 'TURKEY';

	APPEND ROW TO TMPTABLE;
	TMPTABLE_COUNTRY = 'GERMANY';
	TMPTABLE_NAME = 'KARLSRUHE';

	APPEND ROW TO TMPTABLE;
	TMPTABLE_NAME = 'GERMANY';

	APPEND ROW TO TMPTABLE;
	TMPTABLE_COUNTRY = 'GERMANY';
	TMPTABLE_NAME = 'BERLIN';

	APPEND ROW TO TMPTABLE;
	TMPTABLE_COUNTRY = 'TURKEY';
	TMPTABLE_NAME = 'IZMIR';

	/* to sort same level items */
	SORT TMPTABLE ON NAME;
	
	SORT TMPTABLE HIERARCHICAL IDCOLUMN 'NAME' PARENTIDCOLUMN 'COUNTRY';

	/* this command will be discussed later */
	SET TMPTABLE TO TABLE TMPTABLE;
	
Hierarchical sorting is much more useful fo tree tables which is a tree representation of table control. Tree tables will be discussed detailly in next sections, but for now we must know about hierarchical sorting option, because it is an advanced sorting option for all kind of tables.



For any kind of sorting operation, there is no way to relocate all rows to their positions before sort operation. Because TROIA interpreter does not track variable's value history for any type of variable.

Useful Table Commands & Functions
---------------------------------

Removing Rows
=============

To remove rows from a table variable, CLEAR command is used. Clear command has different variations to clear all rows or active row. CLEAR command has different variations but mostly used for table. Here is the syntax which is related with removing rows:

::

	CLEAR ROW | ALL {table};
	
ROW variation of CLEAR command removes the active row without changing the active row cursor, so after the command active row is the new row on same index. If removed row is the last row of a multi row table, new active row is the new last row. Here is the sample code that prints row number and active row before and after the CLEAR ROW, please test different variatins and compare the results. It there is no more rows after clear row active row is set to 0 which is an invalid active row index.

::

	OBJECT:
		TABLE T1,
		STRING STRINGVAR3;

	SELECT USERNAME, PASSW
		FROM IASUSERS WHERE USERNAME LIKE 'L%'
		INTO T1;

	STRINGVAR3 = 'Before: ' + T1_ROWCOUNT + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + 'ActiveRow: ' + T1_ACTIVEROW + TOCHAR(10);
	CLEAR ROW T1;
	STRINGVAR3 = STRINGVAR3 + 'After: ' + T1_ROWCOUNT + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + 'ActiveRow: ' + T1_ACTIVEROW + TOCHAR(10);


With ALL variation, it is possible to remove all rows from table. After clearing all rows active row is set to 0.

::

	OBJECT:
		TABLE T1,
		STRING STRINGVAR3;

	SELECT USERNAME, PASSW
		FROM IASUSERS
		INTO T1;

	STRINGVAR3 = 'Before: ' + T1_ROWCOUNT + TOCHAR(10);
	CLEAR ALL T1;
	STRINGVAR3 = STRINGVAR3 + 'After: ' + T1_ROWCOUNT + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + 'ActiveRow: ' + T1_ACTIVEROW + TOCHAR(10);
	
	
..cleartable..command..
	
Removing Columns
================

In some cases programmers may need remove all columns and data and reconstruct table using APPEND commands (Removing a column also means removing corresponding cell of each row, so removing all columns also removes whole data). To remove all columns DESTROYTABLE command is used and after the DESTROYTABLE command table goes to same state with a table that is newly created by a variable definition command (OBJECT, LOCAL ..), here is the syntax of DESTROYTABLE command:

::

	DESTROYTABLE {table};
	


REMOVECOLUMN

To remove all rows and columns DESTROYTABLE command is used.
	
CLEARTABLE
MODIFY
CONSTRUCT


Some Useful Functions
=====================

SELECTEDROWCOUNT()
GETCOLUMNCOUNT()
	

Data Transfer Between Tables
----------------------------

.move corresponding

.copy table

.merge table
	
Aggregate Commands to Calculate Subtotals
-----------------------------------------

.aggregate

Application Performance and Tables
----------------------------------
.

Sample 1: Compare Traces of LOOP WHERE & LOOP + IF
--------------------------------------------------
.

Sample 2: Compare Execution Times of LOOP Options
-------------------------------------------------
.

Sample 3: Locating on Tables using Hash Index
-------------------------------------------------
.

Sample 4: Sort Users Like a Tree
--------------------------------
.
