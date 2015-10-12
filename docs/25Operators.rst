

=======================
Operators and Expressions
=======================

Operators
--------------------

Operators are the commands which tells the TROIA interpreter to do some mathematical or logical operations using their operands.

Arithmetic Operators
====================

+---------------+---------------------------------+
|   Operator    |   Meaning                       |
+---------------+---------------------------------+
|    **+**      |   add                           |
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
This operators are not supported on if, where or loop statement conditions etc.

Assingment
--------------------

There are two assignment methods in TROIA. First method is using MOVE command, which does not support expressions for source value.

::

	MOVE 'Hello World' TO STRINGVAR1;
	MOVE STRINGVAR1 TO STRINGVAR2;
	MOVE 3 TO INTEGERVAR1;
	
	MOVE 3 + 1 TO INTEGERVAR1;     /* is not valid, expression are not supported */
	MOVE METHOD1() TO INTEGERVAR1; /* is not valid, expression are not supported */

Expressions
--------------------

expressions...


Type Conversion and Casting
---------------------------

type conversion and casting...


Example: Integer Arithmetics
---------------------------

integer arigtmetics example...
