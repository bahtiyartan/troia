

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

Last day of a month is not a conststant, it depends on to the month and date due to whether year is a leap year. For such a kind of calculations TROIA has functions below to ease development efford.

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
	
	
Also is possible to add or substract days, minutes etc to a date and calculate another date. For this kind operatons TROIA has functions like ADDDAYS(), ADDYEARS(), ADDHOURS(), ADDMINUTES(),SUBDAYS(),SUBMONTHS() etc. All this calculations takes timezone, leap year issues and predefined configuration into the account and reduces development efford for TROIA programmers. For more details and functions please see TROIA Help documents.

	
Calculating Date Difference
===========================

To calculate difference between two dates in days or minutes, you must only substract a date from another. This operation returns difference in milliseconds and you can calculate this difference in days or even years. Also TROIA has GETMINUTEDIFF() function that  returns the difference in minutes. Here is an example that shows two different approach about calculating date difference.

::

	OBJECT: 
	 DATETIME DATETIMEVAR1,
	 DATETIME DATETIMEVAR2;

	STRINGVAR3 = '';
	DATETIMEVAR1 = '25.11.1984 03:00:00';
	DATETIMEVAR2 = '25.11.1984 04:00:00';

	LONGVAR1 = (DATETIMEVAR2 - DATETIMEVAR1) / (1000*60);
	LONGVAR2 = GETMINUTEDIFF(DATETIMEVAR1, DATETIMEVAR2);

What is NULLDATE?
-----------------

In TROIA dialogs, DATETIME and DATE textfields can be be leaved as empty. In this case the value of this date/datetime symbols is set to a special value. This special value is called as NULLDATE and this value converted to string as "  .  .       :  :  " or "  .  .    " for date symbols. This approach is also same for table columns for date/datetime columns. To check whether given text is NULLDATE or not TROIA has a NULLDATE() function that returns a boolean (integer) result. Here is a simple example:

::

	OBJECT:
	 STRING STRINGVAR1,
	 STRING STRINGVAR2,
	 STRING STRINGVAR3,
	 DATETIME DATETIMEVAR1;

	DATETIMEVAR1 = '';
	STRINGVAR1 = DATETIMEVAR1;
	STRINGVAR2 = SYS_CURRENTDATE;

	STRINGVAR3 ='';
	STRINGVAR3 = STRINGVAR3 + NULLDATE(STRINGVAR1) + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + NULLDATE(STRINGVAR2) + TOCHAR(10);



Min Date & Max Date Concepts
----------------------------

In some cases, TROIA programmers need some special dates values like upper and lower limits of TROIA dates. Assume that you have an expiration date for a document and in some documents you must use a maximum date value for the documents that never expire. For this cases system returns minimum and maximum dates with SYS_MINDATE and SYS_MAXDATE system variables which are datetime. Also it is possible to check whether a datetime/date symbol is max date/min date or not using ISMAXDATE() and ISMINDATE() functions.

::

	OBJECT:
	 STRING STRINGVAR3;

	STRINGVAR3 = SYS_MINDATE + ' : ' + ISMINDATE(SYS_MINDATE) + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + SYS_CURRENTDATE + ' : ' + ISMINDATE( SYS_CURRENTDATE)+ TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + SYS_MAXDATE + ' : ' + ISMAXDATE( SYS_MAXDATE)+ TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + SYS_CURRENTDATE + ' : ' + ISMAXDATE( SYS_CURRENTDATE)+ TOCHAR(10);

In 3.08.x versions as default, this max date and min date values are 01.01.1975 00:00:00 and 01.01.2030 00:00:00 and they are hardcoded. After 5.01 versions it is possible to set this maximum and minimum years in ServerSettings.ias file of your server with **MinDateYear** and **MaxDateYear** parameters. So it is not recommended that using this hardcode dates inside TROIA code. **Although this parameters are configurable, it is not reccomended to change this values without a planned migration over database tables.**


Basic Date Formatting/Parsing
-------------------------------

Date formatting means converting a date/datetime variable to a string using a format like DD.MM.YYYY or YYYY.MM.DD etc. And parsing is extracting date value from a string. Due to date format resulting value can be different for both formatting and parsing operations.

As default, if a date/datetime variable is assigned to a string variable system uses 'DD.MM.YYYY'/'DD.MM.YYYY HH:MM:SS' format. Same format is valid for parsing dates without providing a date format. So all hardcode dates must be given using this format. Please see the code below and discuss the behavior:

::

	OBJECT: 
	 STRING STRINGVAR3,
	 DATETIME DATETIMEVAR1,
	 DATETIME DATETIMEVAR2;

	DATETIMEVAR1 = '30.12.2018 21:41:42';
	DATETIMEVAR2 = '2018.12.30 21:41:42';

	STRINGVAR3 = DATETIMEVAR1;
	
**This is totally same with decimals, all programmers uses . as decimal separator inside TROIA code. It is hardcoded and independent from language or any localization configuration**
	
	
Formatting Configurations & Related System Variables
====================================================

In troia, there is no need to format date/datetime values for textfield values or table cells. This fields are formatted automatically if they has not a hardcoded special format given on IDE. For controls and table fields to use default formats you must use format texts below:

+-------------------+--------------------------------------------------+
|datetime           | use default datetime format                      |
+-------------------+--------------------------------------------------+
|datetimes          | use default datetime format that contains second |
+-------------------+--------------------------------------------------+
|date               | use default date format                          |
+-------------------+--------------------------------------------------+
|time               | use default time format                          |
+-------------------+--------------------------------------------------+
|times              | use default time format that contains second     |
+-------------------+--------------------------------------------------+

So it is possible to show a DATETIME textfield with default date format using this format parameters. Additionally it is possible to configure textfields and table columns independent from user date format configuration using a date format supported by java like "yy.MM.dd".

 
System calculates default formats for all date related data types using the date formatting configuration on "SYST03 - System Users" transactoin and uses for default formatted textfields and table cells. Also, all this formats can be accessed from TROIA using system variables listed below to format custom texts that uses default date formats.

+-------------------+----------------------------------------------+
|SYS_DATETIMEFORMAT | Default datetime format                      |
+-------------------+----------------------------------------------+
|SYS_DATETIMESFORMAT| Default datetime format that contains second |
+-------------------+----------------------------------------------+
|SYS_DATEFORMAT     | Default date format                          |
+-------------------+----------------------------------------------+
|SYS_TIMEFORMAT     | Default time format                          |
+-------------------+----------------------------------------------+
|SYS_TIMESFORMAT    | Default time format that contains second     |
+-------------------+----------------------------------------------+

::

	OBJECT: 
	 STRING STRINGVAR3;

	STRINGVAR3 = '';
	STRINGVAR3 = STRINGVAR3 + SYS_DATETIMEFORMAT + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + SYS_DATETIMESFORMAT + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + SYS_DATEFORMAT + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + SYS_TIMEFORMAT + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + SYS_TIMESFORMAT + TOCHAR(10);
	
	
This system variables are supported after 5.02.05 and following versions, so it is not possible to use this variables on 3.08.x and 5.01.x versions.

Date Formatting & Parsing Dates with TROIA
==========================================

It is possible to parse and format date related types using default formats or different hard codded formats thanks to FORMATDATE() and PARSEDATE() functions. 

FORMATDATE() function gets a date and format, returns a string due to given format. Here is an examples:

::

	OBJECT: 
	 STRING STRINGVAR3;

	STRINGVAR3 = '';
	STRINGVAR3 = STRINGVAR3 + FORMATDATE(SYS_CURRENTDATE, 'yyyy.MM.dd') + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + FORMATDATE(SYS_CURRENTDATE, SYS_DATETIMEFORMAT) + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + FORMATDATE(SYS_CURRENTDATE, SYS_TIMESFORMAT)+ TOCHAR(10);


PARSEDATE() function gets a string variable and format, returns and datetime variable. Here is an example:

::

	OBJECT: 
	 STRING STRINGVAR3;

	STRINGVAR3 = '';
	STRINGVAR3 = STRINGVAR3 + PARSEDATE('2018.06.19', 'yyyy.MM.dd') + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + PARSEDATE('19.06.2018 17:25', SYS_DATETIMEFORMAT) + TOCHAR(10);
	STRINGVAR3 = STRINGVAR3 + PARSEDATE('17:25:54', SYS_TIMESFORMAT)+ TOCHAR(10);


This system functions are supported after 5.02.05 and following versions, so it is not possible to use this variables on 3.08.x and 5.01.x versions.


Database & Date Format
----------------------

Date formats can be configured for each user, but on database layer only one date/datetime format is used. This format is configured on Database section of "SYST06 - System Parameters" transaction. System automatically formats date related variables and table cells for database interactions without any TROIA level efford.

To format a date/datetime variable using db date format you must use GETDBDATESTR() function. GETDBDATESTR() function is mostly used for preparing database queries that contains hardcode date/datetime values. Please run the code below and compare the result with your database date format configuration.

::

	OBJECT: 
	 STRING STRINGVAR3;

	STRINGVAR3 = GETDBDATESTR(SYS_CURRENTDATE);



Timezone
--------

...

Work Calendar
-------------

In TROIA, it is also possible to define some business layer calendars and make some date calculations like adding days hours considering constraings of this calendars. This kind of date calculations are called "Work Calendar", we will discuss Work Calendars in following sections.
