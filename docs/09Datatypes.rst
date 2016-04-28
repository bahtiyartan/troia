

==================================
Built-in Data Types and Structures
==================================

*Like other programming languages, TROIA has built-in data types and all these types have some specific features. This section aims to introduce all primitive and complex data types. Details about complex data types will be discussed in related sections.*


Built-in Data Types
-------------------

Built-in data types are listed below:

::

	INTEGER        (Ex: 5, default value is: 0)
	LONG           (Ex: 12312312, default value:0)
	DECIMAL        (Ex: 1.32 , all floating point numbers, default value:0.0)
	BOOLEAN        (0 or 1, default value:0)
	
	DATETIME       (Ex: 29.10.1923 16:30:30, default value:definition time) 
	DATE           (Ex: 23.04.1920, default value:definition date) 
	TIME           (Ex: 21:30)
	TIMES          (Ex: 21:30:13)
	
	STRING         (Ex: 'Hello World', default value is empty string)
	STRINGBUILDER  (default value is empty string)
	
	TABLE
	VECTOR

BOOLEAN data type is supported on 5.01 and following releases, so usually INTEGER type is used instead of BOOLEAN type as 1 or 0.
	
TROIA does not support low level types like byte, char, short, float etc.
	
	
StringBuilder & String
----------------------

STRINGBUILDER is a string like symbol type which is designed for only string building operation.

Using a STRINGBUILDER variable instead of STRING increases string concatenation performance dramatically.
To get higher performance on string concatenation, you must use APPENDSTRING command instead of '+' operator.

Although there is not any other distinctive feature between STRINGBUILDER and STRING, it is not recommended to use STRINGBUILDER type instead of STRING on regular string opreations except string concateantion.

Both STRING and STRINGBUILDER types don't have hard-coded maximum length. 

Single quote (') is used to limit hardcode strings.


Table
--------------------

TABLE is the most important build in type of TROIA programming language. TABLE type represents a two dimensional in memory structure similar to database tables.
We will discuss TABLE Type, related commands and functions in next sections, in detail.


Vector
--------------------

VECTOR is a kind of one dimensional array type that is able to store different types of variables.
VECTOR Type, related commands and functions will be discussed in next sections, in detail.


Keywords and Identifiers
------------------------

As mentioned in previous sections TROIA is a command based language. As a result of it's structure TROIA has more reserved words compared to an ordinary programming language.
Basically, TROIA keywords are consisted from command, function and predefined system variable names. 

It is not allowed to use these words as variable and functions name.
To avoid using TROIA keywords as variable/function name programmers must not use the word that has extra color in TROIA Editor.

Some of the most subtle/used TROIA identifiers are listed below.

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
