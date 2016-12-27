

=======================
Custom TROIA Components
=======================

*Its possible to define custom business layer components and use instances of these components on different dialogs or other components. This sections aims to show how "custom components" are defined and used.*


Introduction
------------

While developing dialog user interfaces, programmers use ui items such as buttons, text fields, labels (ui controls). But in some cases, programmers may need to reuse a set of controls on different dialogs to avoid copying and pasting existing codes on different dialogs. A custom TROIA component is a reusable custom ui control which contains one or more simple controls to perform a specific action. Custom components are a type of TROIA development items such as dialogs or classes. Custom TROIA component is called as "component", shorty.

For instance, for a fligh reservation application you may need two date chooser one is for departure date and second one is returning departure date and you have to make a set of validity checks whether inserted dates are valid or correct order etc. If you write whole required code on a dialog, you have to controls troia code when you need a similar control set for an hotel reservation application. Using component infrastructure you define your date chooser as a component and use it on different dialogs.

How Components Stored?
======================

As it is obvious, components have controls, control events and methods. Therefore a component is similar to dialog from the aspect of items that they have. Similar to dialogs component data is stored on development tables which are already used for storing dialogs. This tables are SYSDIALOGS, SYSDLGTEXTS, SYSDLGCODES, SYSDLGFUNCTEX, SYSCONTROLS, SYSCTLTEXTS. For more  information about the roles of this table, please see "Dailog Basics/How Dialogs are Stored?" section.

Defining Components
-------------------

Similar to other troia development items, components are defined using TROIA IDE, too. To create a new component, you must click **TROIA IDE-> New -> New Component** on menu and fill the new component form which is very similar to new dialog form.

.. figure:: images/components/newcomponent.png
   :width: 360 px
   :target: images/components/newcomponent.png
   :align: center
   
In this form you, must select a hotline (change request) and enter a valid component name. A valid component name is unique and at least four characters length similar to dialogs. 

Inheriting and defining cross references are also supported for components and works in same way with dialogs. Please see the related section to get more informatin about inheriting. 

After you click "OK" button, IDE creates a new component and opens design window. In this design window you can create your component due to your requirements. Here is my sample component that has two date fields on it.

.. figure:: images/components/democomponent1.png
   :width: 700 px
   :target: images/components/democomponent1.png
   :align: center

Using Components
----------------

To use a component on a dialog, you must just drag a "TROIA Component" (1) from toolbox and drop it on dialog (2). And write the component name to "Component" feature of your dragged control (3).

.. figure:: images/components/democomponent2.png
   :width: 700 px
   :target: images/components/democomponent2.png
   :align: center

Components & Scope
------------------

Calling Component Methods
=========================

Nested Components
=================

Component Actions
-----------------

FIRECOMPONENTEVENT Command
==========================

::

	FIRECOMPONENTEVENT {eventname};





	