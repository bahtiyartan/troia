

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

How Dialogs are Stored?
-----------------------

All development information (codes, design etc) about dialogs are stored in database tables.

+-----------------+-------------------------------------------------------+
| SYSDIALOGS      |                                                       |
+-----------------+-------------------------------------------------------+
| SYSDLGTEXTS     |                                                       |
+-----------------+-------------------------------------------------------+
| SYSDLGCODES     |                                                       |
+-----------------+-------------------------------------------------------+
| SYSDLGFUNCTEXTS |                                                       |
+-----------------+-------------------------------------------------------+
| SYSCONTROLS     |                                                       |
+-----------------+-------------------------------------------------------+
| SYSCTLTEXTS     |                                                       |
+-----------------+-------------------------------------------------------+

How Dialogs are Loaded on Runtime?
----------------------------------

With "Convert" operation, binary/compilation data is built. This data contains all kinds of dialog information such as name, size, controls, control positions, functions and events etc. 

"Save" operation writes binary data and dialog texts on given language to a ".dlg". It is obvious that dialog files are language dependent because they contains dialog and control texts for a languge.

Dialog files are stored in a folder named **user file path**. User file path is the folder that user loads all binary information(dialog, class, report) and it is configured for each user on SYST03. Here is the format and location of dialog file.

::
	
	format : {userfilepath}\jdlg\{module}\{languagecode}{dialog}.dlg
	
	Example
	SALT01D001 in English:	{userfilepath}\jdlg\SAL\ET01D001.dlg
	SALT01D002 in Deutsch: {userfilepath}\jdlg\SAL\DT01D002.dlg


how a dialog is loaded.


Basic Dialog Events
--------------------

+--------+---------------------------------------------------------------------------------+
| BEFORE | First event on dialog open, fired after controls defined (dialog is not visible |
+--------+---------------------------------------------------------------------------------+
| AFTER  | Fired after "BEFORE" event, dialog is still not visible.                        |
+--------+---------------------------------------------------------------------------------+
| ONSHOW | Fired after "AFTER" event, when dialog is visible on user interface.            |
+--------+---------------------------------------------------------------------------------+


Basic Controls and Control Events
---------------------------------

what is a troia control...

Button Control
==============

button control and events.

TextField Control
=================

textfield control and events.


Switching Between Dialogs
-------------------------

Opening a dialog.

CALL DIALOG {dialog};

CALL DIALOG WITH LOCATION {x}, {y} SIZE {widht}, {height};

Close a dialog.


Functions & Right Click Menu
----------------------------
right click menu.


Sample 1: Counting Words
------------------------

sample.