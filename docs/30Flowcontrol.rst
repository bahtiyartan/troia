

=======================
Flow Control
=======================

	
IF Statement
--------------------
Like other programming languages, IF statement is the main and most used conditional statement in TROIA language. TROIA Interpreter decides to run or ignore IF block depending on conditional expression result.
Here is the syntax of IF Statement in TROIA:

::

	IF condition THEN
		block
	ENDIF;

As it is obvious on syntax above, there is not any kind of enclosing brackets both for condition and true block, three keywords ( IF, THEN, ENDIF) enclose 'condition' and 'block'.
'condition' is an expression which is a combination of relational and logical operators and function calls. 'block' is another TROIA code block.
Here is an example:

::

	IF A > 1 && A < THIS.GETMAXVALUE(P1) THEN
		RESULT = 'A is between one and max value';
	ENDIF;
	
In TROIA all logical expression is executed even if first condition is enough to calculate logical expression's result, unlike most programming languages. Lets say A is 0 before the IF statement, so A > 1 condition returns false. If one of && (and) operator's operands is false, result is always false. TROIA Interpreter executes A < THIS.GETMAXVALUE() conditon, although condition result is obvious. Behaviour is same for || (or) operator when its first operand is true.
	

IF-ELSE Statement
====================
ELSE keyword is used to define another block that will be executed when 'if condition' is false.

::

	IF A == 1 THEN
		RESULT = 'A equals to one';
	ELSE
		RESULT = 'A is less or more than one';
	ENDIF;

ELSE IF is not supported in TROIA, to run multiple conditions you must use nested if blocks. Here is a valid ELSE IF example:

::

	IF A == 1 THEN
		RESULT = 'A equals to one';
	ELSE
		IF A < 1 THEN
			RESULT = 'A is less than one';
		ELSE
			RESULT = 'A is more than one';
		ENDIF;
	ENDIF;


SWITCH Statement
--------------------

SWITCH statement is another way of defining conditional blocks. Here is the syntax of SWITCH statement in TROIA:

::

	SWITCH item
		CASE value[,value]:
			case block 1
		CASE value:
			case block 2
		.
		.
		CASE value:
			case block n
		DEFAULT:
			default block
	ENDSWITCH;
	

SWITCH Statement allows using only variables as switch item (you can not use an expression) and compares item's text value with string values of cases defined with CASE keyword. If you want to execute same block for more than one CASE values you can list values in a single case using comma (,) as separator. In TROIA, BREAK keyword is not supported and required in SWITCH statements. Here is a simple example of SWITCH statement:

::

	OBJECT: 
		STRING VAR,
		STRING RESULT;

	VAR = '8';

	SWITCH VAR 
	CASE 5:
		RESULT = 'It is five';
	CASE 6:
		RESULT = 'It is six';
	CASE '7','8':
		RESULT = 'It is seven or eight';
	DEFAULT:
		RESULT = 'I do not know what it is.';
	ENDSWITCH;
	


WHILE Loop
--------------------

::

	WHILE condition
	BEGIN
		block
	ENDWHILE;


::

	OBJECT: 
		INTEGER VAR,
		STRING RESULT;

	VAR = 1;
	RESULT = '';

	WHILE VAR < 10 
	BEGIN

		IF VAR % 2 == 0 THEN
			RESULT = RESULT + VAR +':even, ';
		ELSE
			RESULT = RESULT + VAR +':odd, ';
		ENDIF;

		VAR = VAR + 1;
	ENDWHILE;


LOOP Command
--------------------
not implemented...

BREAK & CONTINUE Statements
----------------------------
not implemented...

Sample 1: Factorial
----------------------------

not implemented...

Sample 2: Fibonacci Numbers
----------------------------

not implemented...

Other Looping Options
--------------------------------

not implemented...



