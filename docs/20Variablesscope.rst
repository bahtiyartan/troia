

=======================
Variables and Scope
=======================

	
Scope
--------------------

Simply, **scope** of a variable definition is the part or range of software that definition is valid.

In TROIA there are three levels of scope: local, member, global.

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


Member Scope
====================

If a variable defined as a member variable, it is accessible in all methods of the class that is defined in, so class is the largest boundary for a member variable.

It is not possible to access member variables using only their name outside of the class, because they are defined for each instance of class. 
To access a member variable of a class instance @ operator is used. (It is similar to . operator in other languages, INSTANCE1@XPOS	: XPOS member of a class instance named INSTANCE1)

::
	
	In a class method, a member variable overrides variables that is 
	    defined for a larger scope (global). For example if you have an integer
	    type member variable named XPOS and a global variable with same name,
	    all methods use member when XPOS is accessed to do anything.
	

Global Scope
====================

If a variable defined as global variable, it is accessible from any part of the software. In TROIA, largest scope is global and it is bounded by transaction (application).
In other words, if you define a global variable, you can access it from any dialog or class method/event providing that all items run in a transaction (application).


Defining Variables
--------------------

In TROIA, there are multiple variable definition methods. The first and most common method is using data definition commands such as LOCAL, GLOBAL, OBJECT.

Another common method is using controls on dialogs. When a dialog which has a textfield as a TROIA control, system automatically defines a global string symbol which has same name with textfield.	Each control type/subtype has a corresponding variable type to define automatically. Controls, types and subtypes will be discussed in next sections detailly.

Additionally, there are (limited) special commands which are able to define fixed type global variables (ex:SELECT, TABLE).



Variable Definition Commands
-------------------------------------

GLOBAL Command
====================

global command...

MEMBER Command
====================

member command...


LOCAL & PARAMETERS Command
====================

local & parameters commands...

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


Some Facts About Variable Definitions
------------------------------------------------------------

some facts...