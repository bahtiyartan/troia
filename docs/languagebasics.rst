

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


Language Elements
--------------------

language elements...

Dialog
====================

dialogs...

Class
====================

class...


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

ide...


SYS & DEV Transactions
=========================

sys and dev transactions...


Hotline
------------------------

Hotline is "Change Request" in TROIA Platform. Hotlines are created/managed on 'DEVT06 Hotline Management' transaction (application) and they are stored in database.

It is not allowed to change any TROIA Item(dialog, class etc.) without a change request. 
All development tools ask programmer to select hotline before modification and modifications are logged related with selected hotline.


