

=======================
Variables and Scope
=======================

	
Scope
--------------------

Simply, **scope** of a variable definition is the part or range of software that definition is valid.

In TROIA there are three levels of scope: global, member, local.

Global Scope
====================

If a variable defined as global variable, it is accessible from any part of the software. In TROIA, largest scope is global and it is bounded by transaction (application).
In other words, if you define a global variable, you can access it from any dialog or class method/event providing that all items run in a transaction (application).


Member Scope
====================

If a variable defined as a member variable, it is accessible in all methods of the class that is defined in, so class is the largest boundary for a member variable.

It is not possible to access member variables using only their name outside of the class, because they are defined for each instance of class. 
To access a member variable of a class instance @ operator is used. (It is similar to . operator in other languages, INSTANCE1@XPOS	: XPOS member of a class instance named INSTANCE1)

::
	
	In a class method, a member variable overrides variables that is 
	    defined for a larger scope (global). 
		
For example if you have an integer type member variable named XPOS and a global variable with same name, all methods use member when XPOS is accessed to do anything.
	

Local Scope
====================

If a variable defined as a local variable, it is accessable only in the method/event that it is defined in, so method/event is the largest boundary for a local variable.

In TROIA, it is not possible to define a variable for a narrower scope like a block, so if you define a local variable you can access it anywhere in method/event.

::

	A local variable overrides other variables 
	    which are defined for a larger scope (member, global).
	
Local variables are created for each instance of the method/event like any other language (think on recursive calls).

::

	A static local variable is a variable that is defined for once 
	    and shared by all instances of method. 
	
	Static local variables are not supported in TROIA.	
	
Function parameters are local symbols that, programmer can access them only inside the method using parameter name.


Defining Variables
--------------------

In TROIA, there are multiple variable definition methods. The first and most common method is using data definition commands such as LOCAL, GLOBAL, OBJECT.

Another common method is using controls on dialogs. When a dialog which has a textfield as a TROIA control, system automatically defines a global string symbol which has same name with textfield.	Each control type/subtype has a corresponding variable type to define automatically. Controls, types and subtypes will be discussed in next sections detailly.

Additionally, there are (limited) special commands which are able to define fixed type global variables (ex:SELECT, TABLE).



Variable Definition Commands
-------------------------------------

GLOBAL Command
====================

GLOBAL command defines a global variable with given data type. Defining multiple variables with a single GLOBAL command is allowed, even if data types are different.

::

	/* define a single variable */
	GLOBAL:
		STRING STRINGVAR1;
		
	/* define multiple variables */
	GLOBAL:
		STRING STRINGVAR1,
		STRING STRINGVAR2,
		TABLE TABLEVAR1,
		INTEGER INTVAR,
		MYCLASS MYCLASSREC;


MEMBER Command
====================

MEMBER command defines a member variable which is accessible from all methods of the class. It can be used in only class methods including constructor and others, so using MEMBER command in dialogs, reports (etc.) is a programming error. It is possible to define multiple members variables in a single MEMBER command even if data types are different.

::

	/* define a single member */
	MEMBER:
		STRING STUDENTNAME;
		
	/* define multiple members */
	MEMBER:
		STRING FIRSTNAME,
		STRING LASTNAME,
		TABLE ITEMLIST,
		INTEGER XPOSITION
		MYCLASS MYCLASSREC;
		
All kind of data types such as STRING, INTEGER, TABLE or TROIA classes can be defined as member variable.
		
*TROIA Classes and its constructors (_CONSTRUCTOR & _VARIABLES) will be discussed detailly in next sections.*


LOCAL Command
====================

LOCAL Command defines a local variable with given data type. Similar to other data definition commands it is possible to define multiple variables with a single LOCAL command.

::

	/* define a single local variable */
	LOCAL:
		STRING STRINGVAR1;
		
	/* define multiple local variables */
	LOCAL:
		STRING STRINGVAR1,
		STRING STRINGVAR2,
		TABLE TABLEVAR1,
		INTEGER INTVAR,
		MYCLASS MYCLASSREC;

OBJECT Command
====================

object command...


System Variables
--------------------

System variables are global and predefined variables that stores information about system, user session or some specific actions to use these values on TROIA level.
Most of system variables are read-only and their data types depends on variable's purpose.

Some examples of system variables are listed below, for more please view TROIA Help.

::

	SYS_CURRENTDATE       : Returns long value of now.
	SYS_CLIENT            : Client value that is used while login.
	SYS_LANGU             : Language value that is used while login.
	SYS_USER              : Username of current user.
	SYS_VERSION           : TROIA platform server version.
	SYS_AFFECTEDROWCOUNT  : Number of affected rows after db update/insert/delete.
	SYS_CURRENTDIALOG     : Name of current dailog.
	CONFIRM               : Selected value after a confirm or option message.
	SQL                   : Latest SQL Query that is sent to database.
	
It is not allowed to define variables which have same name with a system variable. Most of them starts with SYS prefix, although there are exceptions such as SQL, CONFIRM etc.


Some Facts About Defining Variables
------------------------------------------------------------

Using undefined variables do not cause compiling errors because of TROIA's structure (data transfer between dialogs). If a variable is used before it is defined, it returns its name as value like a string variable that has same value with its name.

Defining same variable more than once, ...


Variable Definition Conventions
------------------------------------------------------------

+ Defining all variables as global is not a good programming convention, variables must be defined narrowest scope that is possible, to save memory, eleminate possible bugs and readibility.

+ Even if most of existing TROIA codes contain OBJECT command, using GLOBAL, LOCAL and MEMBER instead of OBJECT command is recommended to increase readibility.

+ Defining variables using commands except variable definition commands is not recommended because it reduces readibility.

+ As a TROIA programming convention TROIA codes are written in uppercase, so using uppercase for variable names is recommended.

+ Defining variables that start with 'SYS' prefix is not a recommended naming convention.

+ Although using numbers in variable names is supported, using a number as a first character is not recommended.

+ Defining a variable which has same name with a TROIA command, function or system variable is considered as TROIA coding error.