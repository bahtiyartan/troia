

=======
Classes
=======

Introduction
------------

what is a class.

classes and scope.

Creating/Editing Classes
========================

Classes are defined and edit classes in TROIA IDE and "DEVT00 - Class-Browser" transaction. DEVT00 is an older option to manage TROIA classes but has some extra features such as creating SQL scripts for classes etc. We will focus on TROIA IDE to manage classes.

To create a new class you must click **TROIA IDE->New->Class** on menu and fill the new class form.

.. figure:: images/classes/newclass.png
   :width: 360 px
   :target: images/classes/newclass.png
   :align: center
   
As it is obvious that, your class must have a unique name. It is not a compulsory define a base class for your new class, so you can leave base class field if your new class has not a base. "Short description" is a simple documentation text about your class. "Module" and "Status" are just for documentation issues and have no critical effect on class usage. After you click "OK" button on new class form, IDE creates a new class and opens your new class on object explorer.

To edit your existing classes you must click **TROIA IDE->Open Resource** on menu and find and open your classes. It is obvious that both for defining and editing classes requires an hotline which is a change request.


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

Even if there are some differences compared to regular object oriented programming languages, its possible to inherit TROIA classes and override methods of base class (also its possible for dialogs). Inheritance and cross will be discussed detailly in next sections.
	

Sample 1: Math Operations
-------------------------