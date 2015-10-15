

=======================
Operators and Expressions
=======================

Operators
--------------------

Operators are the commands which tells the TROIA interpreter to do some mathematical,relationala and logical operations using their operands.

Arithmetic Operators
====================

+---------------+---------------------------------+
|   Operator    |   Meaning                       |
+---------------+---------------------------------+
|    **+**      |   add, string concat            |
+---------------+---------------------------------+
|    **-**      |   substract                     |
+---------------+---------------------------------+
|    **\***     |   multiply                      |
+---------------+---------------------------------+
|    **/**      |   divide                        |
+---------------+---------------------------------+
|    **%**      |   module                        |
+---------------+---------------------------------+
|    **^**      |   power                         |
+---------------+---------------------------------+


Relational Operators
====================

+---------------+---------------------------------+
|   Operator    |   Meaning                       |
+---------------+---------------------------------+
|      ==       |   is equal to                   |
+---------------+---------------------------------+
|      !=       |   is not equal to               |
+---------------+---------------------------------+
|      >        |   is greater than               |
+---------------+---------------------------------+
|      >=       |   is greater than or equal to   |
+---------------+---------------------------------+
|      <        |   is less than                  |
+---------------+---------------------------------+
|      <=       |   is less than or equal to      |
+---------------+---------------------------------+


Logical Operators
====================

In TROIA codes, there are two set of logical operators. The first set is about TROIA expressions that is used on if, where, loop etc. conditions.

+---------------+---------------------------------+
|   Operator    |   Meaning                       |
+---------------+---------------------------------+
|      !        |   not                           |
+---------------+---------------------------------+
|      &&       |   and                           |
+---------------+---------------------------------+
|      ||       |   or                            |
+---------------+---------------------------------+

Elements of second logical operator set are OR, AND and NOT. This operators are not elements of TROIA language, they are just used in SQL commands that written inside TROIA codes.
This operators are not supported on if, where or loop statement conditions etc. Although these are not used as logical operators, it is not recommended to define variables with this names.

Assignment
--------------------

There are two assignment methods in TROIA. First method is using MOVE command, which does not support expressions for source value.

::

	MOVE 'Hello World' TO STRINGVAR1;
	MOVE STRINGVAR1 TO STRINGVAR2;
	MOVE 3 TO INTEGERVAR1;
	
	MOVE 3 + 1 TO INTEGERVAR1;      /* not valid, expressions are not supported */
	MOVE THIS.F1() TO INTEGERVAR1;  /* not valid, expressions are not supported */
	


Second assignment method is using = operator. This method also supports expressions in right hand side.

::

	STRINGVAR1 = 'Hello World';
	STRINGVAR1 = STRINGVAR1 + 'Hello World';
	INTEGERVAR1 = 3 * (5 + INTEGERVAR1);
	
	INTEGERVAR1 = REPLACE('abc cde', 'cde', 'xyz');
	INTEGERVAR1 = THIS.F1(4+INTEGERVAR2, 'parameter value', P3);
	

Expressions
--------------------

Expressions can be used on a


Type Conversion and Casting
---------------------------

type conversion and casting...


Example: Integer Arithmetics
---------------------------

integer arigtmetics example...
