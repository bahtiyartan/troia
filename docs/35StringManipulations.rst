

=============================
Strings / String Manipulation
=============================

*String is a kind of sequence of characters/digits and it is one of most common types of programming languages. This section aims to introduce some key features of string type and most used string manipulation functions.*

Some Facts About Strings
------------------------

In TROIA, single quote character (') is used to limit hardcode strings.

There is not hard-coded maximum length (character-limit) of string typed variables and hardcode strings, so if you have trouble about maximum character length, possibly its about your length of your database table, table column or editor length etc.

String variables are able to store all kind of characters and digits. 

As mentioned previous sections, undefined variables are returns their name. Because of t so it seems they are STRING symbols but undefined variable's are not


Special Characters & Escaping
=====================================
escape characters, TOCHAR() function


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

Base64 Encoding/Decoding
========================

Base64 is a kind binary-to-text encoding scheme that represent binary data in an ASCII string format. Resulting string contains only 64 kind of characters which contains only a-z,A-Z,0-9,+,/ characters so the resulting string can be stored on or transferred between 'ASCII only' environments.

To encode strings to Base64 BASE64ENCODE() system function is used, to convert a base64 string to a troia string BASE64DECODE() system function is used. Both encoding and decoding requires an encoding like 'UTF-8' etc to convert string to binary or binary to string. 

Here is an encoding and decoding example:

::

	OBJECT:
		STRING STRVALUE,
		STRING STRBASE64,
		STRING STRDECODED;
    
    STRVALUE = 'Programming With TROIA';   
    STRBASE64 = BASE64ENCODE(STRVALUE, 'UTF-8');
    STRDECODED = BASE64DECODE(STRBASE64, 'UTF-8');

Hashing Strings
===============
hashing.
GETDIGEST()


String Concatenation
--------------------

As default + operator is used to concatenate strings in TROIA, but it is not recommended to use + operator to build large strings because of performance issues.

::

	OBJECT:
		STRING FINALSTRING;
		
	FINALSTRING = 'Hello ' + ' World.';
	
Performance problem of string concatenation is not about only '+' operator, its also about STRING type. To solve performance issues, programmers must use STRINGBUILDER data type which is mentioned in previous sections. Defining a STRINGBUILDER is not different from defining a STRING variable. To append/concat a given string to a STRINGBUILDER, APPENDSTRING command is used. Here is STRINGBUILDER & APPENDSTRING equvalent of previous example.

::

	OBJECT:
		STRINGBUILDER SBFINALSTRING;
		
	APPENDSTRING 'Hello' TO FINALSTRING;
	APPENDSTRING ' World' TO FINALSTRING;
	
STRINGBUILDER is designed for only hi-performance string building operations, so it is not recommended to use STRINGBUILDER object instead of STRING. 

*To benchmark using STRING/+ and STRINGBUILDER/APPENDSTRING you can write a while loop that makes one thousand concat operation.*

Looping on Strings: PARSE Command
---------------------------------

looping on strings.

Sample 1: Basic String Functions
--------------------------------

Here is a simple string functions example, please try to find return values after each function call.

::

	OBJECT:
		STRING STRMESSAGE,
		INTEGER NINDEX;

	STRMESSAGE = ' Hello TROIA ' + TOCHAR(10);

	/* remove whitespaces */
	STRMESSAGE = TRIM(STRMESSAGE);

	/* replace word 'TROIA' with 'world' */
	STRMESSAGE = REPLACE(STRMESSAGE, 'TROIA', 'World');

	/* calculate index of 'world' */
	NINDEX = STRPOS(STRMESSAGE, 'World');

	/* get first 5 character*/
	STRMESSAGE = STRSTR(STRMESSAGE, 0, 5);	


Sample 2: List Numeric Tokens of a String
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



