

=======================
String Manipulation
=======================

	
Basic String Functions/Commands
-------------------------------

basic string functions.

TRIM()
REPLACE()
STRSTR()
STRLIKE()
STRNCOPY()
STRPOS()
ISNUMERIC()
TOCHAR()

Switch Uppercase/Lowercase
==========================
LOWERCASE().
UPPERCASE().

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
---------------------

string concatenation.

stringbuilder type.

appendstring command.


Example: List Numeric Tokens of a String
========================================

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



