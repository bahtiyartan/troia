

=======================
Language Basics
=======================

*This section aims to introduce language and it's basic terms. Most of subtopics will be discussed detailly in next chapters.*


TROIA is a high level programming language which is designed by IAS (Industrial Application Software) for developing business applications.

As a fourth generation programming language (4GL), TROIA has strong abstraction from all hardware specific details, bytes and bits etc.
It has strong ability to operate large collections of information, in a programmer-friendly manner.

TROIA codes are executed by "TROIA Interpreter" which is one of **the main components of Application Server**.

Understanding the Purpose
-------------------------

The main purpose of TROIA Language is developing business applications, so it contains too many useful features to access, operate and report business layer information.

Thanks to these features, TROIA reduces "the pain" caused by technical details of programming languages, so programmers can focus business layer issues.
Timezone, localization, multi-language support, cross platform & database, formatters, validators, api&dlls etc. are the examples of these painful technnical details.


Basic Language Elements
--------------------

Class
====================

TROIA is an object oriented language, so it allows programmerts to define classes to perform a task or model a business entity.
Like any other object oriented language, cleses have members and methods and they support inheritance and encapsulation.

Although TROIA classes able to model any kind of business entity and its behaviors, classes are usually used as a set of methods as a TROIA programming convension.
TROIA Programmers usually use tables to store an entity's fields because of advantages of using tables.

Dialog
====================

Dialog is a **user interface form** designed to perform a task on, like a web form or a swing dialog.
Usually, dialog a is acombination of simple or complex TROIA controls such as textfields, buttons, comboboxes, checkboxes, tables, charts etc.

Dialogs have methods similar to classes, so TROIA programmers define and call methods on dialogs.
Additionally they have predefined events. **Events** are TROIA methods which are called by TROIA runtime when a specific action performed like "form load", "button clicked" etc.

.. figure:: images/languagebasics/dialog.png
   :width: 700 px
   :target: images/languagebasics/dialog.png
   :align: center

Report
====================

report...

Transaction
=========================

Transaction is simply a database record that defines a standalone TROIA application.
The main attributes of a transaction are name, description and start dialog.

When you call/start a transaction, start dialog of this transaction is opened automatically.

TROIA Interpreter
--------------------

interpreter...

Convert/Save
====================

convert/save...


Trace
=========================

trace...


Development Tools
--------------------

development tools...

TROIA IDE
====================

**TROIA IDE** is the primary development tool of TROIA Platform. It's main functionality is defineing/modifing TROIA items such as dialogs, classes, reports and components.
Additionally, it contains useful tools such as optimization tools, code comparing tools, import/export tools etc.

As default users can not access TROIA IDE,to access users must have "DEVELOPMENT" right. Also, "DEVELOPMENT(READ-ONLY)" right allow users to access TROIA IDE and only read/view existing TROIA items. 

SYS & DEV Transactions
=========================

sys and dev transactions...


Hotline
------------------------

Hotline is "Change Request" in TROIA Platform. Hotlines are created/managed on 'DEVT06 Hotline Management' transaction (application) and they are stored in database.

It is not allowed to change any TROIA Item(dialog, class etc.) without a change request. 
All development tools ask programmer to select hotline before modification and modifications are logged related with selected hotline.


