

==================
Parsing XML & JSON
==================

*This sections aims to show xml & json parsing options*


Conversion Between XML & JSON
-----------------------------

Reading XML Structure
---------------------

+-------------+---------------------------------+
| ID          |                                 |
+-------------+---------------------------------+
| PID         |                                 |
+-------------+---------------------------------+
| NAME        |                                 |
+-------------+---------------------------------+
| PATH        |                                 |
+-------------+---------------------------------+
| VALUE       |                                 |
+-------------+---------------------------------+
| LEVEL       |                                 |
+-------------+---------------------------------+
| ISATTRIBUTE |                                 |
+-------------+---------------------------------+


::

	<?xml version="1.0" ?>
	<CustomerList>
		<Sector HeadOrganization="World Techlogy Association">Technology</Sector>

		<Customer CustName="XYZ Industries">
			<City>Tokyo</City>
			<ProductGroup>Integrated Circuits</ProductGroup>
		</Customer>

		<Customer CustName="ABC Technology">
			<City>Madrid</City>
			<ProductGroup>Mobile Applications</ProductGroup>
		</Customer>

		<Customer CustName="IAS Software">
			<City>Istanbul</City>
			<ProductGroup>ERP</ProductGroup>
		</Customer>
	</CustomerList>
	
::

	READXMLSTRUCTURE STRINGVAR3 TO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;



Validating XML Documents
------------------------

Some Useful XML Functions
-------------------------

Formatting XML Documents
========================

::

	OBJECT:
		STRING STRXML,
		STRING STRINGVAR3;

	STRXML = '<ROOT><TEAM><NAME>Team 1</NAME><SCORE>0</SCORE>';
	STRXML = STRXML + '</TEAM><TEAM><NAME>Team 2</NAME>';
	STRXML = STRXML + '<SCORE>1</SCORE></TEAM></ROOT>';

	STRINGVAR3 = WRAPXML(STRXML);
	
::

	<?xml version="1.0" encoding="UTF-8"?><ROOT>
	  <TEAM>
		<NAME>Team 1</NAME>
		<SCORE>0</SCORE>
	  </TEAM>
	  <TEAM>
		<NAME>Team 2</NAME>
		<SCORE>1</SCORE>
	  </TEAM>
	</ROOT>
	

CLEARDOCUMENT() Function
========================

::

	OBJECT:
		STRING STRXML,
		STRING STRINGVAR3;

	STRXML = '<ROOT><TEAM><NAME>Team 1</NAME><SCORE>0</SCORE>';
	STRXML = STRXML + '</TEAM><TEAM><NAME>Team 2</NAME>';
	STRXML = STRXML + '<SCORE>1</SCORE></TEAM></ROOT>';

	STRINGVAR3L = CLEARDOCUMENT(STRXML,'TEAM');

Parsing XML Documents
---------------------

XML Parsing Examples
--------------------

Exercise 1: Parse using READXMLSTRUCTURE
----------------------------------------

Exercise 2: Parsing Simple XML Documents
----------------------------------------

Exercise 3: Parsing Complex XML Documents
-----------------------------------------

Exercise 4: Validation
----------------------







	