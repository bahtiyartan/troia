

=======================
Data Types and Structures
=======================

Like any other programming language, TROIA has primitive and complex data types and structures.

	
Primitive Data Types
--------------------

primitive data types...	
	
STRING
====================

strings in detail...


Main Data Type : Table
--------------------

Some Examples
=============

Some examples of variables and datatypes:

Average of N numbers
--------------------

In the next program we will do an average of N numbers.

::

    #!/usr/bin/env python
    N = 10
    sum = 0
    count = 0
    while count < N:
        number = float(input(""))
        sum = sum + number
        count = count + 1
    average = float(sum)/N
    print("N = %d , Sum = %f" % (N, sum))
    print("Average = %f") % average


The output

::

    $ ./averagen.py
    1
    2.3
    4.67
    1.42
    7
    3.67
    4.08
    2.2
    4.25
    8.21
    N = 10 , Sum = 38.800000
    Average = 3.880000

Temperature conversion
----------------------

In this program we will convert the given temperature to Celsius from Fahrenheit by using the formula C=(F-32)/1.8

::

    #!/usr/bin/env python3
    fahrenheit = 0.0
    print("Fahrenheit Celsius")
    while fahrenheit <= 250:
        celsius = ( fahrenheit - 32.0 ) / 1.8 # Here we calculate the Celsius value
        print("%5.1f %7.2f" % (fahrenheit , celsius))
        fahrenheit = fahrenheit + 25

The output

::

    $ ./temperature.py
    Fahrenheit Celsius
    0.0  -17.78
    25.0   -3.89
    50.0   10.00
    75.0   23.89
    100.0   37.78
    125.0   51.67
    150.0   65.56
    175.0   79.44
    200.0   93.33
    225.0  107.22
    250.0  121.11

Multiple assignments in a single line
=====================================

You can even assign values to multiple variables in a single line, like

::

    >>> a , b = 45, 54
    >>> a
    45
    >>> b
    54

Using this swapping two numbers becomes very easy

::

    >>> a, b = b , a
    >>> a
    54
    >>> b
    45

To understand how this works, you will have to learn about a data type called *tuple*. We use *comma* to create tuple. In the right hand side we create the tuple (we call this as tuple packing) and in the left hand side we do tuple unpacking into a new tuple.

Below we have another example of tuple unpacking.

::

    >>> data = ("Kushal Das", "India", "Python")
    >>> name, country, language = data
    >>> name
    'Kushal Das'
    >>> country
    'India'
    >>> language
    'Python'


Keywords and Identifiers
========================

The following identifiers are used as reserved words, or keywords of the language, and cannot be used as ordinary identifiers. They must be spelled exactly as written here:

::

    CONFIRM    class      finally    is         return
    None       continue   for        lambda     try
    True       def        from       nonlocal   while
    and        del        global     not        with
    as         elif       if         or         yield
    assert     else       import     pass
    break      except     in         raise