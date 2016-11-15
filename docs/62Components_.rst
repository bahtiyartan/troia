

=======================
Custom TROIA Components
=======================

*Its possible to define custom business layer components and use instances of these components on different dialogs or other components. This sections aims to show how "custom components" are defined and used.*


Introduction
------------

While developing dialog user interfaces, programmers use ui items such as buttons, text fields, labels (ui controls). But in some cases, programmers may need to reuse a set of controls on different dialogs to avoid copying and pasting existing codes on different dialogs. A custom TROIA component is a reusable custom ui control which consists of simple controls to perform a specific action. Custom components are a type of TROIA development types such as dialogs or classes. Custom TROIA component is called as "component", shorty.

For instance, for a fligh reservation application you may need two date chooser one is for departure date and second one is returning departure date and you have to make a set of validity checks whether inserted dates are valid or correct order etc. If you write whole required code on a dialog, you have to copy them when you need a similar control set for an hotel reservation application.

, programmers creates custom ui components that contains more than one ui control and use them in different applications.

How Components Stored?
======================

Components & Scope
------------------

Defining Components
-------------------	

Using Components
----------------

Calling Component Functions
===========================

Nested Components
=================

Component Actions
-----------------

FIRECOMPONENTEVENT Command
==========================

::

	FIRECOMPONENTEVENT {eventname};





	