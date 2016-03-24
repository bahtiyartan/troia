

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
		
Another option is using a table control on a dialog. Every ui table has a global table variable with same name. This table variable is data model of ui table, and programmers access table data over this model. This is also same with a primitive data types and dialog controls. 

Unlike primitive data types (int, double etc), table is a two dimensional data type. Therefore, only variable definition is not so much useful for tables.In addition to defining table instance, also we must define its structure/columns to store data.

select command.


Adding Rows To Tables
---------------------
. add rows to table.


Accessing Table Data
--------------------


ActiveRow
=========

Using Row Index
===============


Data Transfer Between Tables
----------------------------

.move corresponding

.copy table

.merge table


Table Flags
-----------
.


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