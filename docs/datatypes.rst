

=======================
Built-in Data Types and Structures
=======================

Like any other programming language, TROIA has primitive and complex data types and structures.

	
Primitive Data Types
--------------------

Basic built-in data types are listed below:

::

	INTEGER        (Ex: 5)
	LONG           (Ex: 12312312)
	DECIMAL        (Ex: 1.32 , all floating point numbers)
	BOOLEAN        (0 or 1, commonly INTEGER type is used instead of BOOLEAN type)
	
	DATETIME       (Ex: 29.10.1923 16:30:30) 
	DATE           (Ex: 23.04.1920) 
	TIME           (Ex: 21:30)
	TIMES          (Ex: 21:30:13)
	
	STRING         (Single quote (') is used to limit hardcode strings)
	STRINGBUILDER
	
	TABLE
	VECTOR

TROIA, does not support low level types like byte, char, short, float etc.	
	
	
StringBuilder & String
--------------------

STRINGBUILDER is a string like symbol type which is designed for only string building operation.

Using a STRINGBUILDER variable instead of STRING increases string concatenation performance dramatically.
To get higher performance on string concatenation, you must use APPENDSTRING command instead of '+' operator.

Although there is not any other distictive feature between STRINGBUILDER and STRING, it is not recomended to use STRINGBUILDER type instead of STRING on regular string opreations except string concateantion.

Both STRING and STRINGBUILDER types don't have hardcoded maximum length.


Table
--------------------

TABLE is the most important build in type of TROIA programming language. TABLE type represents a two dimensional in memory structure similar to database tables.
We will study on TABLE Type, related commands and functions in next sections detailly.


Vector
--------------------

VECTOR is a kind of one dimensional array type that is able to store different types of variables.
We will study on VECTOR Type, related commands and functions in next sections, detailly.


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
	PUT            BEGINTRAN      COMMITTRAN     ROLLBACKTRAN   BOOLEAN
	STRINGBUILDER  
