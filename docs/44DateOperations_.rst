

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
---------------------------------------

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

There are too many possibilities casting date related symbols, please write your own codes to understand the main approach. It is also usefull to review "Operators and Expressions" section for more information. We will focus on relation between long and date related data types.


In background, all date related types are stored as long, so it is also possible to make type conversion/castion operations between date related types and long/integer variables. Long value of a date symbol is millisecond value starts from 01.01.1970 00:00:00 (actually it is a little bit more complicated because of timezone issues). Although it is possible to assing a date related variable to a long or long variable to a date related variable is possible, TROIA has MILLISECONDSTODATE() and DATETOMILLISECONDS() system functions to handle same operations. Here is an example about long and datetime data type, please discuss the results for better understanding:

::

	OBJECT: 
	 STRING STRINGVAR3,
	 DATETIME DATETIMEVAR1,
	 DATETIME DATETIMEVAR2,
	 DATETIME DATETIMEVAR3,
	 LONG LONGVAR1,
	 LONG LONGVAR2;

	DATETIMEVAR1 = '25.11.1984 13:00:00';

	LONGVAR1 = DATETOMILLISECONDS(DATETIMEVAR1);
	LONGVAR2 = DATETIMEVAR1;

	DATETIMEVAR2 = MILLISECONDSTODATE(LONGVAR1);
	DATETIMEVAR3 = MILLISECONDSTODATE(LONGVAR1);

	STRINGVAR3 = LONGVAR1 + TOCHAR(10) + LONGVAR2;

Some Useful Functions and Variables
-----------------------------------


Getting Current Date
====================

A date related variable's initial value is the time that it is initialized/created, therefore it is possible to read the current date just creating a date or datetime variable. But getting current date with this approach creates a vulnerability about date related bugs, so it is not reccommended to use this option.

The safest and most correct way of getting current date is reading SYS_CURRENTDATE system variable's value. SYS_CURRENTDATE is a datetime system variable and returns current time in each value read. Here is a sample code about using SYS_CURRENTDATE:

::

	OBJECT: 
	 INTEGER INTEGERVAR1,
	 STRING STRINGVAR3;

	STRINGVAR3 = '';
	INTEGERVAR1 = 0;
	STRINGVAR3 = 'Data Type:' + GETVARTYPE(SYS_CURRENTDATE) + TOCHAR(10);

	WHILE INTEGERVAR1 < 3 
	BEGIN
		DELAY 1000;
		STRINGVAR3 = STRINGVAR3 + SYS_CURRENTDATE + TOCHAR(10);
		INTEGERVAR1 = INTEGERVAR1 + 1;
	ENDWHILE;

Another option is using CURRENTTIMEMILLIS() system function that returns current time as long value. Although it is mostly used to measure the time between two operations, it can be also used for getting current date. The example below combines two differant usages of CURRENTTIMEMILLIS() function.

::

	OBJECT: 
	 LONG LONGVAR1,
	 DATETIME DATETIMEVAR1,
	 STRING STRINGVAR3;

	DATETIMEVAR1 = CURRENTTIMEMILLIS();

	LONGVAR1 = CURRENTTIMEMILLIS();
	DELAY 1000;
	LONGVAR1 = CURRENTTIMEMILLIS() - LONGVAR1;

	STRINGVAR3 = DATETIMEVAR1 + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + 'It takes: '+ LONGVAR1 + ' ms.'+TOCHAR(10);
	
	
	
Checking and Validating Date Related Types
==========================================

In some cases, programmers need to know whether a variable is a date or datetime symbol. ISDATE() system function checks given variable and returns a boolean result. The example below shows that this function returns true (1) for only DATE and DATETIME data types, otherwise it returns false (0) even variable contains a string that can be convertable to date:

::

	OBJECT: 
	 STRING SRINGVAR1,
	 DATE DATEVAR1,
	 DATETIME DATETIMEVAR1,
	 TIME TIMEVAR1,
	 TIMES TIMESVAR1;

	STRINGVAR1 = DATETIMEVAR1;
	STRINGVAR3 = '';

	STRINGVAR3 = STRINGVAR3 + 'SRINGVAR1 :' + ISDATE(SRINGVAR1) + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + 'DATEVAR1 :' + ISDATE(DATEVAR1)+ TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + 'DATETIMEVAR1 :' + ISDATE(DATETIMEVAR1)+ TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + 'TIMEVAR1 :' + ISDATE(TIMEVAR1)+ TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + 'TIMESVAR1 :' + ISDATE(TIMESVAR1)+ TOCHAR(10);


To validate a string value whether it is a valid date/datetime TROIA has CHECKDATE() system function that gets a string parameter and returns boolean (integer). Similarly, CHECKTIME() is used for checking time/times validity. Here is a sample code that shows the behaviour of CHECKDATE() and CHECKTIME() functions.

::

	OBJECT: 
	 STRING SRINGVAR1,
	 DATE DATEVAR1,
	 DATETIME DATETIMEVAR1,
	 TIME TIMEVAR1,
	 TIMES TIMESVAR1;

	STRINGVAR1 = DATETIMEVAR1;
	STRINGVAR3 = '';

	STRINGVAR3 = STRINGVAR3 + 'SRINGVAR1 :' + CHECKDATE(STRINGVAR1) + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + '19.06.2018 :' + CHECKDATE('19.06.2018')+ TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + '35.06.2018 :' + CHECKDATE('35.06.2018')+ TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + 'DATETIMEVAR1 :' + CHECKDATE(DATETIMEVAR1)+ TOCHAR(10);

	STRINGVAR3 = STRINGVAR3 + 'TIMEVAR1 :' + CHECKTIME(TIMEVAR1)+ TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + 'TIMESVAR1 :' + CHECKTIME(TIMESVAR1)+ TOCHAR(10);


Extracting Data From Date/Datetime Variables
============================================

It is possible to get date or time part of given date variable using GETDATE() and GETTIME() functions. It is also possible to get same parts assigning a DATETIME variable to a DATE, TIME or TIMES variable. TROIA automatically extracts correct part, please see the casting section for more information. The example below shows GETDATE() and GETTIME() function's behavior.

::

	OBJECT: 
	 STRING SRINGVAR1,
	 DATETIME DATETIMEVAR1;

	STRINGVAR1 = DATETIMEVAR1;
	STRINGVAR3 = '';

	STRINGVAR3 = STRINGVAR3 + GETDATE(STRINGVAR1) + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + GETTIME(STRINGVAR1) + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + GETDATE(DATETIMEVAR1)+ TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + GETTIME(DATETIMEVAR1)+ TOCHAR(10);

	
To get a single part of a date like year, day or month you must use the functions in the like the example below:

+-------------------+--------------------------------------------+
|GETDAY()           | Returns the day value of given date.       |
+-------------------+--------------------------------------------+
|                   | Returns day of week. In other words is     |
|GETDAYOFWEEK()     | given day monday, tuesday or ...           |
|                   | 1:monday, 2: tuesday 3:wednesday  ...      |
+-------------------+--------------------------------------------+
|GETHOUR()          | Returns the hour part                      |
+-------------------+--------------------------------------------+
|GETMINUTE()        | Returns the minute part                    |
+-------------------+--------------------------------------------+
|GETMONTH()         | Returns the month part                     |
+-------------------+--------------------------------------------+
|GETYEAR()          | Returns the year part                      |
+-------------------+--------------------------------------------+
|GETWEEK()          | Returns week number of given date          |
+-------------------+--------------------------------------------+


::

	OBJECT: 
	 STRING SRINGVAR1,
	 DATE DATEVAR1,
	 DATETIME DATETIMEVAR1;

	STRINGVAR1 = DATETIMEVAR1;
	STRINGVAR3 = '';

	STRINGVAR3 = STRINGVAR3 + GETDAYOFWEEK(STRINGVAR1) + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + GETDAYOFWEEK(DATETIMEVAR1)+ TOCHAR(10);
	
	STRINGVAR3 = STRINGVAR3 + GETWEEK(STRINGVAR1) + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + GETWEEK(DATETIMEVAR1)+ TOCHAR(10);



Calculating Dates
=================

+-------------------+--------------------------------------------+
|GETDATEFROMWEEK()  | Gets week and year parameter and returns   |
|                   | the first day of the given week as date    |
+-------------------+--------------------------------------------+
|FIRSTDATEINMONTH() | Gets month and year parameter and returns  |
|                   | the first day of the given month as date   |
+-------------------+--------------------------------------------+
|LASTDATEINMONTH()  | Gets month and year parameter and returns  |
|                   | the last day of the given month as date    |
+-------------------+--------------------------------------------+

Here is a simple example that returns the fists day of this week:

::

	OBJECT: 
	 INTEGER THISYEAR,
	 INTEGER THISWEEK;

	STRINGVAR3 = '';

	THISYEAR = GETYEAR(SYS_CURRENTDATE);
	THISWEEK = GETWEEK(SYS_CURRENTDATE);
	
	STRINGVAR3 = GETDATEFROMWEEK(THISWEEK,THISYEAR);

	
Calculating Date Difference
===========================

+-------------------+--------------------------------------------+
|GETMINUTEDIFF()    |                                            |
+-------------------+--------------------------------------------+


+-------------------+--------------------------------------------+
|GETDBDATESTR()     |                                            |
+-------------------+--------------------------------------------+


	
	




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















	