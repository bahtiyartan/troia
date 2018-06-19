

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















	