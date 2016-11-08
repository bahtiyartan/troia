

===================
Inheritance & Cross
===================

*Inheritance is one of the key concepts of object oriented programming. This sections aims to introduce inheritance and relelated concepts.*


What is "Inheritance"?
--------------------
#what is inheritance


Inheritance on TROIA
--------------------
#inheritance on troia


Inheritance on Classes
======================
#inheritance on classes


Inheritance on Dialogs/Components
=================================
#inheritance on dialogs/componentss


Using SUPER() Method
--------------------
#using super method


Using SUPER Object
------------------
#using super object


What is "Cross"?
---------------

A cross simply a record which indicates that class/dialog loaders will use another class/dialog **instead of** crossed dialog/class. For example if you define a cross from class A to class B, system loads class B instead of A. Cross is a database record and it is read and applied on runtime, in other words it is not a part of development process. With crosses; there is no need to chage/modify standart application, so customer customizations need less efford and time.

Crosses are mostly used with an inherited class that changes structure/behavior of a base class or adds new functionalities to a base class. Assume that we have CAT class which have a MOVE method. This method increases the x position five by five. Here is the pseudocode of the structure and method of CAT class.

::
	
	class CAT:
		MEMBER:
			INTEGER X;
		
		function MOVE
			X = X + 5;
			RETURN;
	
	/* this code is not compilable, 
	   it is just for assumption */
			
and in somewhere of the standart application, an instance of CAT class is defined and its move method is called like below:
::

	OBJECT:
		CAT RECCAT;
	
	RECCAT.MOVE();
	
And assume again CATs on your company are lazy and move one by one. To solve this case you must find all CAT definitions and change them to your child LAZYCAT class or do something like a factory pattern to decide which cat type will be created. With cross concept you don't need to find and change all definitions. If you define a cross from CAT to your LAZYCAT class, system laods LAZYCAT instead of CAT class in all applications. Although cross is mostly used from a base item to a child item, it is possible to define a cross between independent classes (but please think on possible problems about crossing independent items).

It is possible to define crosses for dialogs, classes, reports and components. Cross definitions for classes usually called "class cross" and others are called "dialog cross".


Cross Levels & Loading Order
----------------------------

It is possible to define crosses in two level: "**system cross**" and "**user cross**". A system cross is system wide and if you define a system cross, this cross is valid for all users that connects to same database. User crosses are defined for a user or a profile, so this kind of crosses are valid for a user or users defined in a profile.

System; firstly reads system crosses. After system crosses; user crosses are read starting with deepest profile, and finally user's own crosses are read. Latest cross owerwrites previous cross definitions. In other words, priority order is user's own crosses, user's profile crosses, system crosses. Also it is possible to define crosses as chain here is a chain cross sample:

::

	A -> B
	B -> C
	C -> D
	
Cross information is loaded while user logging in, so crosses that defined/deleted while user online are ignored until user login again.

To remove a cross you can define a cross from item's itself (A -> A). But defining crosses as an infinite loop in more than one step is considered as TROIA level error and this may cause stackowerflow error or infinite loop on login. Here is an invalid cross definition:

::
	
	A -> B
	B -> C
	C -> A

How to Define Crosses
---------------------
#define system level cross
#related database table

#define user level cross
#related database table



Example 1: Understanding Cross Order
------------------------------------

Assume a U1 user whose user profile is P1. Cross definitions are like below:

::

	SYSTEM : A -> B
	SYSTEM : C -> D
	SYSTEM : E -> F
	SYSTEM : G -> H
	P1 : A->X
	P1 : C->C
	U1 : A->Y
	U1 : F->Z
	
What is the final cross table for the user?
	
	
	
	







	