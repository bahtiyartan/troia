

=============
Dialog Basics
=============

Introduction
------------

what is a dialog.

dialogs and scope.

types of dialogs

Creating/Editing Dialogs
========================
define..

How Dialogs are Stored?
-----------------------

All development information (codes, design etc) about dialogs are stored in database tables.

+-----------------+-------------------------------------------------------+
| SYSDIALOGS      | Stores dialog information such as size, name etc.     |
+-----------------+-------------------------------------------------------+
| SYSDLGTEXTS     | Stores caption of dialog related with language.       |
+-----------------+-------------------------------------------------------+
| SYSDLGCODES     | Stores events & methods of dialog and dialog controls |
+-----------------+-------------------------------------------------------+
| SYSDLGFUNCTEXTS | Strores the caption of methods related with language. |
+-----------------+-------------------------------------------------------+
| SYSCONTROLS     | Stores control information such as size, position etc.|
+-----------------+-------------------------------------------------------+
| SYSCTLTEXTS     | Stores captions of controls related with language.    |
+-----------------+-------------------------------------------------------+

Binary Format of Dialogs
-------------------------

With "Convert" operation, binary/compilation data is built. This data contains all kinds of dialog information such as name, size, controls, control positions, functions and events etc. 

"Save" operation writes binary data and dialog texts on given language to a ".dlg". It is obvious that dialog files are language dependent because they contains dialog and control texts for a languge.

Dialog files are stored in a folder named **user file path**. User file path is the folder that user loads all binary information(dialog, class, report) and it is configured for each user on SYST03. Here is the format and location of dialog file.

::
	
	format : {userfilepath}\jdlg\{module}\{languagecode}{dialog}.dlg
	
	Example:
	SALT01D001 in English> {userfilepath}\jdlg\SAL\ET01D001.dlg
	SALT01D002 in Deutsch> {userfilepath}\jdlg\SAL\DT01D002.dlg


System reads dialog file when dialog is opened. On runtime system does not access development tables. 



Basic Dialog Events
--------------------

Events are predefined methods that called by system automatically when a specific action occurs. For example; "button clicked" is an event called automatically when user click a button.
Programmers must implement the event to do something on related action occurs.

Here is the most used events of dialogs:

+--------+---------------------------------------------------------------------------------+
| BEFORE | First event on dialog open, fired after controls defined (dialog is not visible |
+--------+---------------------------------------------------------------------------------+
| AFTER  | Fired after "BEFORE" event, dialog is still not visible.                        |
+--------+---------------------------------------------------------------------------------+
| ONSHOW | Fired after "AFTER" event, when dialog is visible on user interface.            |
+--------+---------------------------------------------------------------------------------+

Additionally, dialogs have ONTIMER, TRANSCALLED, BEFOREEXTENSION events which are called for some specific actions. This events will be discussed on related sections.


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