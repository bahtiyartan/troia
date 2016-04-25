

==================
Parsing XML & JSON
==================

*This sections aims to show xml & json parsing options*


Conversion Between XML & JSON
-----------------------------

Convert XML to JSON
===================

::

	CONVERTXMLJSON( {input}, 'JSON');
	
::

	<menu id="file" value="File">
		<popup>
			<menuitem value="New" onclick="CreateNewDoc()" />
			<menuitem value="Open" onclick="OpenDoc()" />
			<menuitem value="Close" onclick="CloseDoc()" />
	  </popup>
	</menu>
	
::
	
	OBJECT:
		STRING STRJSON,
		STRING STRINGVAR3;

	STRJSON = CONVERTXMLJSON(STRINGVAR3, 'JSON');
	STRINGVAR3 = STRJSON;
	
.. code-block:: json

	{
	  "@id": "file",
	  "@value": "File",
	  "popup":   [
			{
			"@value": "New",
			"@onclick": "CreateNewDoc()"
		},
			{
			"@value": "Open",
			"@onclick": "OpenDoc()"
		},
			{
			"@value": "Close",
			"@onclick": "CloseDoc()"
		}
	  ]
	}

Convert JSON to XML
===================

::

	CONVERTXMLJSON( {input}, 'XML'[, 'NOTYPEHINT' | 'JSONPREFIX'] );

.. code-block:: json

	{"menu": {
	  "id": "file",
	  "value": "File",
	  "popup": {
		"menuitem": [
		  {"value": "New", "onclick": "CreateNewDoc()"},
		  {"value": "Open", "onclick": "OpenDoc()"},
		  {"value": "Close", "onclick": "CloseDoc()"}
		]
	  }
	}}
	
::

	OBJECT:
		STRING STRXML,
		STRING STRINGVAR3;

	STRXML = CONVERTXMLJSON(STRINGVAR3,'XML','NOTYPEHINT');
	STRINGVAR3 = STRXML;
	
.. code-block:: xml

	<?xml version="1.0" encoding="UTF-8"?>
	<o>
	   <menu>
		  <id>file</id>
		  <popup>
			 <menuitem>
				<e>
				   <onclick>CreateNewDoc()</onclick>
				   <value>New</value>
				</e>
				<e>
				   <onclick>OpenDoc()</onclick>
				   <value>Open</value>
				</e>
				<e>
				   <onclick>CloseDoc()</onclick>
				   <value>Close</value>
				</e>
			 </menuitem>
		  </popup>
		  <value>File</value>
	   </menu>
	</o>



Reading XML Structure
---------------------

::

	READXMLSTRUCTURE {xmlcontent} TO {targettable};

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
		<ListInfo>
			<CreatedAt>10.10.2015</CreatedAt>
			<CreatedBy>btan</CreatedBy>
		</ListInfo>
		
		<Customer Name="XYZ Financial Solutions">
			<City>Tokyo</City>
			<ProductGroup>Payment Systems</ProductGroup>
		</Customer>

		<Customer Name="ABC Technology">
			<City>Madrid</City>
			<ProductGroup>Mobile Applications</ProductGroup>
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







	