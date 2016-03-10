

=======
Classes
=======

Introduction
------------

what is a class.

classes and scope.

Creating/Editing Classes
========================
define classs..

How Classes Stored and Laoded?
==============================

how a dialog stored.

how a dialog is loaded.


Defining Class Instances
------------------------
define instance...

Basic Class Methods
--------------------

Classes has two predefined methods named _VARIABLES, _CONSTRUCTOR, this methods qare called when an variable definition command defines a new class instance. They are also called "class initializer" methods and mostly like constructors on other object oriented programming languages.

As a programming convention _VARIABLES method is used for defining members and other required variables and _CONSTRUCTOR method is used to build internal structures of class instance. But there is not a technical constraint to use them different purposes. Also it is possible to call function initializers manually but it is not a recommended.

The only difference between regular methods and class initializers are about variable definitons made by OBJECT command. For details about OBJECT command and scope issue, please read the related sections of this book.

Calling Class Methods
---------------------

Classes also have methods that can be called from outside of the class over an class instance. There is a not a special syntax for calling a TROIA Class method. Most important part while calling a class method is specifying class instance name, because each instance can have an internal state. Here is a simple example of calling a class method:

::

	OBJECT:
		MATHTEST CLASSINSTANCE,
		INTEGER RESULT;
		
	RESULT = CLASSINSTANCE.SUM(5, 6);
	
It is also possible to define class methods as recursive and call other class methods. To call call a class method inside class THIS keyword is used, because developer of class is not able to possible instances of class. Here is a simple example:


::

	/* this is a class method code, which returns a text */
	PARAMETERS:
		INTEGER PA,
		INTEGER PB;
	
	LOCAL:
		INTEGER MAXNUM;
	
	/* class have another method named MAX */
	MAXNUM = THIS.MAX(PA, PB);
	
	RETURN 'Maximum number is ' + MAXNUM;
	

Class Inheritance
-----------------
..
	

Sample 1: Math Operations
-------------------------