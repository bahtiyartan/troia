

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
	
	/* sample code, there is not any %s in message text, 
	so there is not a paremeter after WITH keyword	*/
	
	MESSAGE BAS C100 WITH;
	
This syntax is simples and most used syntax of MESSSAGE command, for more details such as options and default option please see related help documents.

Message texts are defined in "SYST02 - System Messages" transaction using message definition parameters. This parameters are the module of messsage and an message id which is simple long number. MESSAGE command finds the appropriate message text and options using module, id and user's login language. 

In "SYST02 - System Messages" transaction, it is possible to define message texts and options in multiple languages. Message texts can contain '%s' string formatting character for the parameters which will be provided on runtime using WITH keyword on MESSAGE command. Also it is possible to define indexes for text parameters like '%s1', '%s2' for if the word order changes language by language.


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

Reading User Response & System Variables
========================================
CONFIRMATION, OPTION or PARAMETER message types, we need to read which option is selected or the text input by user. User response is set to CONFIRM and SYS_CONFIRMTEXT system variables, so programmers can read the value and do something due to user input. In confirmation messages, CONFIRM system variable is set to YES when user selects yes option, otherwise it is set to NO. 

::

	MESSAGE BAS C100 WITH;

	IF CONFIRM == 'YES' THEN
		STRINGVAR1 = 'User said: yes!';
	ELSE
		STRINGVAR1 = 'User said: no!';
	ENDIF;

In option messages, CONFIRM variable is set to an integer which shows the order of selected option and SYS_CONFIRMTEXT is set to text value of selected option.

Another system variable which is set on MESSAGE command is SYSMESSAGE. SYSMESSAGE is set to message text for programmers who need message text. Remember message text is calculated using given module, messageid, login language and applied message parameters. 

Message on Batch/Server Transactions
------------------------------------
As it is obvious, messages shown on client side and needs a client side interaction, so while interpreter running a message command, it returns client and waits for a client interaction. This case is called "code breaking" and simply considered as "using a client side resource while code execution in server side". 

In batch transactions, there is no user that is able to interact or answer message dialog, therefore intepreter behavior is different on batch and inserver (INSERVER) transactions. In this kind of server only code executions, system automatically answer messages with default option, confirms (YES) all confirmation messages and ignores information/warning messages. Additionally inserts all messages to IASBATCHERR system table which is able to store message, session and server information.

Here is an example how to get your messages from IASBATCHERR table on a batch transaction:

::

	SETBATCHTRAN TRUE;

	MESSAGE BAS E110 WITH;
	MESSAGE BAS E110 WITH;
	MESSAGE BAS E110 WITH;

	SELECT *
		FROM IASBATCHERR
		WHERE CLIENTCONNID = SYS_CLIENTCONNECTIONID 
			AND TRANSID = SYS_TRANSACTIONID
		INTO TMPTABLE;

	SET TMPTABLE TO TABLE TMPTABLE;
	SETBATCHTRAN FALSE;


Message on Database Transactions
--------------------------------
Messages on "database transactions" (between BEGINTRAN-COMMITTRAN) are answered automatically by server and sent to client side as information messages after "database transaction" closed. All messages emerged in "database transaction" are transferred to client as a bulk information message. Messages on sql transactions are stored in SYSBATCHMESSAGES system variable whose type is table. It will be discussed detailly in database access section.

Here is a sample code that shows how to get messages from SYSBATCHMESSAGES table which is a system variable:

::

	SETSERVERONLY(1);

	MESSAGE BAS E110 WITH;
	MESSAGE BAS E110 WITH;
	MESSAGE BAS E110 WITH;

	COPY TABLE SYSBATCHMESSAGES INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;

	SETSERVERONLY(0);


Other Alerting Options
----------------------
In TROIA, there are some other options to point out controls or create some popups to take users attention. Please see help documents of ATTENTION and ALERT commands which have different user interfaces and interaction methods.


Exercise 1: Reading Message Text/Answer
-------------------------------------

- Define an option message.
- Define a confirmation which contains format text (%s)
- Add a button which shows option message first and prints selected message to your second confirmation message text.
- Print your confirmation message text and answer to a textfield.
