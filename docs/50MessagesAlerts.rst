

===================
Messages and Alerts
===================

MESSAGE Command
---------------

Programmers use popup messages to inform users or get some critical data such as confirmation, selecting an option etc.

.. figure:: images/messages/confirmmessage.png
   :width: 350 px
   :target: images/messages/confirmmessage.png
   :align: center

To show such messages in TROIA, "MESSAGE" command is used. MESSAGE command gets some parameters that defines the message instead of message text's itself (because of multi language support). Here the basic syntax of MESSAGE command and sample code that shows messagebox above:

::
	
	MESSAGE {module} {messagetype}{messageid} WITH {inputparamsformessagetext};
	
	/* sample code */
	MESSAGE BAS C100 WITH;
	
This syntax is simples and most used syntax of MESSSAGE command, for more details such as options and default option please see related help documents.

Message texts are defined in "SYST02 - System Messages" transaction using message definition parameters. This parameters are the module of messsage and an message id which is simple long number. MESSAGE command finds the appropriate message text and options using module, id and user's login language.


Message Types
=============

Message Type effects message appearence on user interface and the way how user will intract with message such as selecting an option, making a text input or just clicking ok button on an information message. All message types are listed below:

+-------------+--------+-----------------------------------+
| **Type**    |**Code**|                                   |
+-------------+--------+-----------------------------------+
| INFORMATION |   I    | Information icon on message popup |
+-------------+--------+-----------------------------------+
| ERROR       |   E    | Error icon on message popup       |
+-------------+--------+-----------------------------------+
| WARNING     |   W    | Warning icon on message popup     |
+-------------+--------+-----------------------------------+
| CONFIRMATION|   C    | Yes/No Questions                  |
+-------------+--------+-----------------------------------+
| OPTION      |   O    | Custom options                    |
+-------------+--------+-----------------------------------+
| PARAMETER   |   P    | User Keyboard Input               |
+-------------+--------+-----------------------------------+


Message Texts & Translations
============================
message texts


Reading User Response & System Variables
========================================
system variables

Code Breaking & Store
---------------------
code breaking


Other Alerting Options
----------------------
alerting options.
