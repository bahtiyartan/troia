

=======================
Data Types and Structures
=======================

Like any other programming language, TROIA has primitive and complex data types and structures.

	
Primitive Data Types
--------------------

::

	STRING
	INTEGER
	LONG
	DECIMAL
	DATETIME
	DATE
	TIME
	TIMES

	BOOLEAN
	STRINGBUILDER


StringBuilder
--------------------

STRINGBUILDER is a string like symbol type which is designed for only string building operation.

Using a STRINGBUILDER variable instead of STRING increases string concatenation performance dramatically.
To get higher performance on string concatenation, you must use APPENDSTRING command instead of '+' operator.

Although there is not any other distictive feature between STRINGBUILDER and STRING, it is not recomended to use STRINGBUILDER type instead of STRING on regular string opreations except string concateantion.


Table
--------------------

table in detail...


Vector
--------------------

vector in detail...


Keywords and Identifiers
------------------------

As mentioned in previous sections TROIA is a command based language. As a result of it's structure TROIA has more reserved words compared to an ordinary programming language.
Basically, TROIA keywords are consisted from command, function and predefined system variable names. 

It is not allowed to use these words as variable and functions name.
To avoid using TROIA keywords as variable/function name programmers must not use the word that has extra color in TROIA Editor.

Some of the most subtle/used TROIA idendifiers are listed below.

::

	GET            IF             MARKERSET      RETURNVALUE    COPY
	EXECUTE        MESSAGE        SELECT         MARKERPOINT    RETURN
	BREAK          DECIMAL        INSERT         MOVE           SELECTED
	BREAKPOINT     DELETE         INSERTED       NOTFOUND       SET              
	CALL           DELETED        ZOOMID         NOTSELECTED    SQL
	CLASS          DEQUE          JAVA           NULL           ZOOM
	CLEAR          ENQUE          VOID           PAGENUM        WHILE
	CONFIRM        ENTITYID       LOOP           RETURN         SWITCH
	TRUE           TIMES          FALSE          UPDATED        ZOOMFYI
	PUT            BEGINTRAN      COMMITTRAN     ROLLBACKTRAN   
