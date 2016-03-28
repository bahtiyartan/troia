

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


ActiveRow
=========

Using Row Index
===============


Table Flags
-----------
.

Data Transfer Between Tables
----------------------------

.move corresponding

.copy table

.merge table



Sorting & Aggregate
-------------------

sort.

aggregate.
	

Locating on Table
-----------------
.
	
Looping on Tables
-----------------

loop.

build hashindex.	
	
	
Basic Table Operations
----------------------

.

Application Performance and Tables
----------------------------------
.

Sample 1: ..
---------------------------------