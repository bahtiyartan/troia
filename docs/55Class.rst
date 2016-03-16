

=======
Classes
=======

*In object oriented programming, a class is an extensible structure that has some features (members), behavior (methods) and an initial state. As an object oriented programming language, TROIA also supports classes. This section aims to introduce concept an its key features in TROIA.*

Introduction
------------

Briefly, TROIA classes are software structures that have behaviors as methods, features as members, and constructors to set initial states. With these features TROIA classes are very similar to most used object oriented language's classes (such as java, c# etc). When it comes to programming practices, TROIA classes have some different aspects from other object oriented languages.

In object oriented languages, usually all structures (such as user interface forms, form elements) are also classes which are included in framework's itself and programmers able to extend this classes and/or implement new classes. In TROIA, dialogs, reports and other iu items are not considered as class. TROIA classes are same level structures with dialogs or reports. In other words, dialogs are not classes.

Additionally, even if TROIA classes are able to store member fields to represent features, they are considered as "sets of methods" that adds some behavior. Because TROIA has an "alternative structure" to classes for storing object's features. It is obvious that this "alternative structure" is "table" which will be discussed next sections. But we must emphasize that there is not a technical constraint to use class members to represend real-life objects, its just about programming practices.


Creating/Editing Classes
========================

Classes are defined and edit classes in TROIA IDE and "DEVT00 - Class-Browser" transaction. DEVT00 is an older option to manage TROIA classes but has some extra features such as creating SQL scripts for classes etc. We will focus on TROIA IDE to manage classes.

To create a new class you must click **TROIA IDE->New->Class** on menu and fill the new class form.

.. figure:: images/classes/newclass.png
   :width: 360 px
   :target: images/classes/newclass.png
   :align: center
   
As it is obvious that, your class must have a unique name. It is not a compulsory define a base class for your new class, so you can leave base class field if your new class has not a base. "Short description" is a simple documentation text about your class. "Module" and "Status" are just for documentation issues and have no critical effect on class usage. After you click "OK" button on new class form, IDE creates a new class which contains two emtpty basic methods and opens your new class on object explorer.

To edit your existing classes you must click **TROIA IDE->Open Resource** on menu and find and open your classes. It is obvious that both for defining and editing classes requires an hotline which is a change request.


How Classes Stored and Loaded?
------------------------------

There are two types of information that defines a class. Here is the tables that stores all development information classes.

+------------+---------------------------------------------------------------------+
| SYSCLSHEAD | Stores basic information of class such as class name, base class    |
+------------+---------------------------------------------------------------------+
| SYSCLSFUNC | Stores class methods information such as codes, return type         |
+------------+---------------------------------------------------------------------+

When you convert a class, TROIA interpreter reads these two tables, compiles the code and saves binary class files with .cls extension to user's class path. Only these files are read when class instances created, in other words when a class defined system does not access database tables that stores class information and codes.

Basic Class Methods
--------------------

Classes has two predefined methods named _VARIABLES, _CONSTRUCTOR, this methods are called when an variable definition command defines a new class instance. They are also called "class initializer" methods and mostly like constructors on other object oriented programming languages.

As a programming convention _VARIABLES method is used for defining members and other required variables and _CONSTRUCTOR method is used to build internal structures of class instance. But there is not a technical constraint to use them different purposes. Also it is possible to call function initializers manually but it is not a recommended.

The only two main difference between regular methods and class initializers:

- This methods are called automatically on class instance definiton.
- OBJECT Command's behaviour is different compared to its behaviour on regular class methods. 

For details about OBJECT command and scope issue, please read the related sections of this book.


Defining Class Instances
------------------------
Defining a class instance is as simple as defining an integer variable. The only difference is using your new type instead of "INTEGER". Here is an instance definition of "MATHTEST" class and it is same for all data definition commands such as GLOBAL, OBJECT, LOCAL etc.

::

	OBJECT:
		MATHTEST MATHREC;
	
When you defining a class instance you must consider all variable defintion points which are discussed previous sections. Additionally, when a variable definition command executed to define a class instance, class initilalizer methods are fired. But this methods are fired only once, in other words even if same instance are defined more than once class initilizers are fired only once. This rule is same both for same or different command instances. To understand the behavior correctly please see the trace of code below:


::

	OBJECT:
		INTEGER NINDEX;

	NINDEX = 0;

	WHILE NINDEX < 2
	BEGIN
		OBJECT:
			RDTA AREC;
			
		OBJECT:
			RDTA AREC;
			
		NINDEX = NINDEX + 1;
	ENDWHILE;


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
	

Accessing Class Members
-----------------------
.
	

Class Inheritance
-----------------

Even if there are some differences compared to regular object oriented programming languages, its possible to inherit TROIA classes and override methods of base class (also its possible for dialogs). Overriding class initilizer methods is not supported, if overriding method and base method is executed as if they are a single constructor.  

Inheritance, both for dialogs and classes will be discussed detailly in next sections.
	

Sample 1: Math Operations
-------------------------

Define a class that:

- has a integer member 'factor' value whose default value is 1.
- has a method SUM method, calculates sum of given two parameters and returns sum * factor.

Create two instances of your class and set different 'factor' values for two instances and compare results.


Sample 2: Defining Unexisting Class Instances
---------------------------------------------

Try to create an class instance using OBJECT command with an undefined class name and see the trace.