

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

	IF A > 0 && A < THIS.GETMAXVALUE(P1) THEN
		RESULT = 'A is between zero and max value';
	ENDIF;
	
In TROIA all logical expression is executed even if first condition is enough to calculate logical expression's result. Lets say A variable is -1 before the IF statement A > 0 condition returns false. If one of && (and) operator's operands is false, result is always false. Most used programming languages do not execute A < THIS.GETMAXVALUE() conditon, but in TROIA it is executed. Behaviour is same for || (or) operator when its first operand is true.
	



IF-ELSE Statement
====================
not implemented...

SWITCH Statement
--------------------
not implemented...

WHILE Loop
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



