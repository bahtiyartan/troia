

=============================
Strings / String Manipulation
=============================

	
Basic String Functions/Commands
-------------------------------

to get information about strings

+-------------+------------------------------------------------------+
| STRLEN      | Returns the length of given string                   |
+-------------+------------------------------------------------------+
| STRPOS()    | Returns index of given string inside another         |
+-------------+------------------------------------------------------+
| ISNUMERIC() | Check string whether all characters are numeric      |
+-------------+------------------------------------------------------+
| STRLIKE()   | Compares string with given pattern like SQL LIKE op. |
+-------------+------------------------------------------------------+


to manipualte strings

+-------------+------------------------------------------------------+
| TRIM()      | Removes whitespaces from the begining and the end.   |
+-------------+------------------------------------------------------+
| REPLACE()   | Replaces a given string with another                 |
+-------------+------------------------------------------------------+
| STRSTR()    | Returns substring of string with index and length    |
+-------------+------------------------------------------------------+
| LOWERCASE() | Returns lowercase of given string with given language|
+-------------+------------------------------------------------------+
| UPPERCASE() | Returns uppercase of given string with given language|
+-------------+------------------------------------------------------+


For more string manipulation functions or more information about these functions (like parameter order and return types etc), please see String Manipulation section of TROIA Help.

Special Characters / Escape Charaters
=====================================
escape characters, TOCHAR() function


BASE64 Encoding/Decoding
========================
base64.
BASE64ENCODE()
BASE64DECODE()

Hashing Strings
===============
hashing.
GETDIGEST()



Looping on Strings: PARSE Command
---------------------------------

looping on strings.


String Concatenation
--------------------

string concatenation.

stringbuilder type.

appendstring command.


Sample 1: List Numeric Tokens of a String
-----------------------------------------

::

	OBJECT:
		STRINGBUILDER SB,
		STRING STRNEWLINE,
		STRING MAINSTRING,
		STRING TOKEN;

	SB = '';
	STRNEWLINE = TOCHAR(10);
	MAINSTRING = '1|A|2|B|3|C|4|D|5|E';

	PARSE MAINSTRING INTO TOKEN DELIMITER '|'
	BEGIN

		IF !ISNUMERIC(TOKEN) THEN
			CONTINUE;
		ENDIF;

		APPENDSTRING TOKEN TO SB;
		APPENDSTRING ' is a number.' TO SB;
		APPENDSTRING STRNEWLINE TO SB;
	ENDPARSE;



