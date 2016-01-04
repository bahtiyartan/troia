

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
------------------------------

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

With "convert" and "save" operations, binary information is built and stored on dialog files with .dlg extension. .dlg file contains all kind of information about dialog such as name, size, controls, control positions, functions, events and texts. Dialog files are language dependent and they are read only runtime (not in development time).

Dialog files are stored in file path of user which logged on system. User file path is the folder that user loads binary files of all TROIA items and it is configured for each user on SYST03.

::

	Dialog file location and name:
	
	{userfilepath}\jdlg\{module}\{languagecode}{dialog}.dlg
	
	For SALT01D001 and English:
	
	{userfilepath}\jdlg\SAL\ET01D001.dlg


how a dialog is loaded.


Basic Dialog Events
--------------------

+---------+---------------------------------------------------------------+
| BEFORE  |                                                               |
+---------+---------------------------------------------------------------+
| AFTER   |                                                               |
+---------+---------------------------------------------------------------+
| ONSHOW  |                                                               |
+---------+---------------------------------------------------------------+


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