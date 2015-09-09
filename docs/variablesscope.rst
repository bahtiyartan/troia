

=======================
Variables and Scope
=======================

	
Scope
--------------------

Simply, **scope** of a variable definition is the part or range of your application that definition is valid.

In TROIA there are three levels of scope: local, member, global.

Local Scope
====================

Member Scope
====================

Global Scope
====================


Data Definition Commands
--------------------

definition commands...

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
