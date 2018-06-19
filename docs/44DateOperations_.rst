

==========================
Working With Date/Datetime
==========================

*This section aims to introduce date/datetime/time types, related configurations,structures and concepts to ease date handling/manipulation*

Date Data Types
---------------

In contrary to some other programming languages, TROIA has more than one data type to store and manipulate date, datetime,time variables. We know about data types from previous sessions, but to refresh our knowledge about this types here is the list of related data types:

::

	DATE           (Ex: 23.04.1920, default value:definition date)
	DATETIME       (Ex: 29.10.1923 16:30:30, default value:definition time) 
	
	TIME           (Ex: 21:30)
	TIMES          (Ex: 21:30:13)
	
As it is obvious, main difference between DATE and DATETIME type is time part which consisted by hour,minute and second. TIME and TIMES data types are used for storing only time information. Their main difference is second part. TIMES data type is relatively a new data type that is supported after 5.01.01 releases(not supported on 3.08.x releases.)


Due to your application's requirements you must use the most primitive data type. It is not recommended that storing a date information in a DATETIME symbol and 00:00:00 as its time part.


Type Conversion & Casting on Date Types
=======================================

We know that TROIA handles type casting operations in background. This rule is also valid for date related data types, so it is possible to assign date,datetime, time, times variable any other typed variable except decimal, or assing any variable to any date data type. 

Here is a simple code block that contains four casting operation:

::

	OBJECT: 
	 STRING STRINGVAR3,
	 DATETIME DATETIMEVAR1,
	 DATE DATEVAR1,
	 DATETIME DATETIMEVAR2,
	 TIME TIMEVAR1;

	STRINGVAR3 = '25.11.1984 13:00:00';
	DATETIMEVAR1 = STRINGVAR3;

	DATEVAR1 = DATETIMEVAR1;
	DATETIMEVAR2 = DATEVAR1;

	TIMEVAR1 = DATETIMEVAR1;
	
In this example, first one is assigning a string to a datetime variable. This operation contains a kind of parsing operation we will discuss parsing and formatting dates in following sections detailly. Second one is assingning datetime to date, in this operation only date part is transferred to target variable. The third one is date to datetime, in this case system uses 00:00:00 for the time part of target symbol. The last one is from datetime to time, In this case only time part of source variable transferred to target symbol.

There are too many possibilities casting date related symbols, please write your own codes to understand the main approach. It is also usefull to review "Operators and Expressions" section for more information about type casting.

In background, all date related types are stored as long. 

MILLISECONDSTODATE()
DATETOMILLISECONDS()

ISDATE()
CHECKDATE()

Getting Current Date
====================

CURRENTTIMEMILLIS() 
SYS_CURRENTDATE

What is NULLDATE?
-----------------

...

NULLDATE()

Min Date & Max Date Concepts
----------------------------

SYS_MAXDATE
SYS_MINDATE

ISMAXDATE()
ISMINDATE()


Basic Date Formatting
---------------------

...

Date Formatting & Parsing Dates with TROIA
==========================================

FORMATDATE()
PARSEDATE()

...


Date Formatting Configurations
==============================

...

System Variables about Date Formatting
======================================

SYS_DATETIMEFORMAT
SYS_DATETIMESFORMAT
SYS_DATEFORMAT
SYS_TIMEFORMAT
SYS_TIMESFORMAT


Basic Date Functions
--------------------

GETDATE()
GETDATEFROMWEEK()
GETDAY()
GETDAYOFWEEK()
GETDBDATESTR()
GETHOUR()
GETMINUTE()
GETMINUTEDIFF()
GETMONTH()
GETTIME()
GETWEEK()
GETYEAR()
FIRSTDATEINMONTH()
LASTDATEINMONTH()




Some Facts About Date/Datetime and Related Data Types
-----------------------------------------------------

...

Database & Date Format
======================

...

Timezone
========

...

Work Calendar
========

...















	