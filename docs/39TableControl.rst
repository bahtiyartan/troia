

===============================
Table on Dialog : Table Control
===============================

.

Introduction
------------

.

Column Information & Labels
---------------------------


Table Events
-----------------------

|RowInsert         | |
|RowDelete         | |
|RowUndelete       | |
|CellChangeBefore  | |
|CellChangeAfter   | |
|ZoomAfter         | |
|ZoomBefore        | |
|ColumnDoubleClick | |
|Copy              | |
|Paste             | |
|ColumnSort        | |
|Drag              | |
|Drop              | |
|RightClickMenu    | |
|ExpandBefore      | |
|ArrowClick        | |
|ButtonCellClick   | |


Sorting UI Tables
-----------------

As mentioned before all table controls has a table variable as its model on server side. So there is no difference between sorting a ui table and a table variable. All SORT command variations are supported for table variables that is created as a ui table model. 

Additionally users are able to sort table data over right click menu of table column header. In right click menu there are three options for "ascending sorting", "descending sorting" and "clear sorting info". Although sorting over right click menu is an user level operation and its not directly related with development process, programmers is able to add some behaviour when user sorts table over column right click menu thanks to **ColumSort** event. This event is fired after sorting operation, so programmers must consider that table's data is sorted on event code. Also some system variables are set befere the event, to help programmers read some useful information about the sorting operation. Here is the list of system variables which set before ColumnSort event:

+------------------+----------------------------------------------------------------+
| SYS_SORTEDTABLE  | Name of sorted last sorted table.                              |
+------------------+----------------------------------------------------------------+
| SYS_SORTEDCOLUMN | Comma separated list of sorted columns                         |
+------------------+----------------------------------------------------------------+
| SYS_SORTED       | Comma separated list of sort columns an directions (asc, desc).|
+------------------+----------------------------------------------------------------+

**SYS_SORTED** variable returns columns and sorting directions in SORT command's format (ASC USERNAME, DESC CREATEDBY), so programmers is able to reuse this data on dynamic sort operations. Here is a sample **ColumnSort** event code, that prints variables anda table data for "DEVT11 - Runcode Test Transaction".

::

	STRINGVAR3 = 'SYS_SORTEDTABLE: ' + SYS_SORTEDTABLE + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 +'SYS_SORTED: ' + SYS_SORTED + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + 'SYS_SORTEDCOLUMN: ' + SYS_SORTEDCOLUMN + TOCHAR(10);

	STRINGVAR3 = STRINGVAR3 + 'ROWS' + TOCHAR(10) + '---------' + TOCHAR(10);
	LOOP AT TMPTABLE
	BEGIN
		STRINGVAR3 = STRINGVAR3 + TMPTABLE_USERNAME + TOCHAR(10);
	ENDLOOP;



Tree Tables
-----------
tree table and related flags.


Pivot View and Pivot Configurations
-----------------------------------
.

Chart View and Chart Configurations
-----------------------------------
.

Other Useful Features
---------------------

Report Wizard & Templates
=========================

.

Filtering Rows
==============
. FILTERED flag.

Conditional Formatting
======================
.


Aggregate and Aggregate Commands
================================

.


Sample 1: Colouring Rows & Row ToolTip
--------------------------------------


Sample 2: Hiding Rows
---------------------

