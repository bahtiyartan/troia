

=============
Dialog Basics
=============

Introduction
------------

what is a dialog.

dialogs and scope.

Creating/Editing Dialogs
========================
define..

How Dialogs Stored and Loaded?
==============================

All development information (codes, design etc) about dialogs are stored in database tables. 

+-------------+-------------------------------------------------------+
| SYSDIALOGS  |                                                       |
+-------------+-------------------------------------------------------+
| SYSCONTROLS |                                                       |
+-------------+-------------------------------------------------------+
|             |                                                       |
+-------------+-------------------------------------------------------+

With "convert" and "save" operations, binary information is built and stored on *.dlg files. *.dlg file contains all kind of information about dialog such as size, controls and control positions, functions, events, texts etc and they are only read on runtime (they are not used on development time).

.dlg files are stored in file path of user which logged on system. User file path is the folder that user loads binary files of all TROIA items and it is configured for each user on SYST03.

how a dialog is loaded.


Basic Dialog Events
--------------------

dialog events and functions.


Basic Controls and Control Events
---------------------------------

basic controls.


Switching Between Dialogs
-------------------------
switching.


Functions & Right Click Menu
----------------------------
right click menu.


Sample 1: Counting Words
------------------------

sample.