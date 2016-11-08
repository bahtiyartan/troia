

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

A cross simply a record which indicates that class/dialog loaders will use another class/dialog **instead of** crossed dialog/class. For example if you define a cross from class A to class B, system loads class B instead of A. It is possible to define crosses for dialogs, classes, reports and components. A cross is a database record and it is read and applied on runtime, in other words it is not a part of development process. With crosses; there is no need to chage/modify standart application, so customer customizations need less efford and time.

Crosses are mostly used with an inherited class that changes structure/behavior of a base class or adds new functionalities to a base class. For example assume we have CAT class which have a MOVE method and this method increases the x position five by five. Here is the pseudocode of the structure and method of CAT class.

::
	/* this code is not compilable, just for assumption */
	class CAT:
		MEMBER:
			INTEGER X;
		
		function MOVE
			X = X + 5;
			RETURN;
			
and assume that in somewhere of the standart application, an instance of CAT class is defined and its move method is called like below:
::

	OBJECT:
		CAT RECCAT;
	
		RECCAT.MOVE();
	
And assume again CATs on your company are lazy and moves one by one. To solve this case you must find all CAT definitions and change them to your child LAZYCAT class or do something like a factory pattern to decide which cat type will be created. With cross concept you don't need to find and change all definitions. If you define a cross from CAT to your LAZYCAT class, system laods LAZYCAT instead of CAT class in all applications. Although cross is mostly used from a base dialog/ to a child dialog/class, it is possible to define a cross between independent classes (please think on possible problems about different interfaces of independent classes).

It is possible to define crosses in two level: "**system level cross**" and "**user level cross**". A system level cross is system wide,in other words if you define a system level cross, cross is valid for all users that connects to same database. User level crosses are defined for a user or a profile, so this kind of crosses are valid for a user or users defined in a profile.

#loading order

Cross information is loaded while user logging in, so crosses that defined/deleted while user online are ignored until user login again.



How to Define Crosses
---------------------
#define system level cross
#related database table

#define user level cross
#related database table








	