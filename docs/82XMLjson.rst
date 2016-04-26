

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
	
.. code-block:: xml

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

Validating XML Documents
------------------------

::

	VALIDATEXML {validatorpath} {xmlpath} WITH {valtype} [TO {details}];
	VALIDATEXML TEXT {validatortext} {xmlastext} WITH {valtype} [TO {details}]; 
	
	
.. code-block:: xml

	<?xml version="1.0" encoding="UTF-8"?>
	<schema xmlns="http://purl.oclc.org/dsdl/schematron">

	<ns uri="http://www.topologi.com/example" prefix="ex"/>
		
	<pattern name="Check structure">
		<rule context="ex:Person">
			<assert test="@Title">
				Person must have title
			</assert>
			<assert test="count(ex:Name)=1 and count(ex:Gender)=1">
				Person should have Name / Gender
			</assert>
			<assert test="ex:*[1] = ex:Name">
				Name must appear before element Gender
			</assert>
		</rule>
	</pattern>	
	<pattern name="Check co-occurrence constraints">
		<rule context="ex:Person">
			<assert test="(@Title = 'Mr' and ex:Gender = 'Male') 
			   or @Title != 'Mr'">
				If the Title is "Mr", gender must be "Male"
			</assert>
		</rule>
	</pattern>
	
	</schema>

.. code-block:: xml

	<ex:Person Title="Mr" xmlns:ex="http://www.topologi.com/example">
		<ex:Name>Eddie</ex:Name>
		<ex:Gender>Male</ex:Gender>
	</ex:Person>

	
	
::

	OBJECT: 
		STRING STRXMLPATH,
		STRING STRSCHPATH
		STRING STRINGVAR3;

	STRXMLPATH = 'xml.xml';
	STRSCHPATH = 'validator.sch';

	COPYFILE '*C:\TMP\xml.xml' INTO STRXMLPATH;
	COPYFILE '*C:\TMP\validator.sch' INTO STRSCHPATH;

	VALIDATEXML STRSCHPATH STRXMLPATH WITH SCHEMATRON TO STRINGVAR3;
	STRINGVAR1 = SYS_STATUS + ' ' + SYS_STATUSERROR;
	
Parsing XML Documents
---------------------


XML Parsing Examples
--------------------

Example 1: Using Auto Parser
============================

.. code-block:: xml

	STRINGVAR3 = '<?xml version="1.0" ?>
	<CustomerList>
		<Customer>IAS Software</Customer>
		<Customer>XYZ Technology</Customer>
		<Customer>ABC Design</Customer>
	</CustomerList>';

	OBJECT: 
	 TABLE CUSTOMERS;

	PARSEXML TEXT STRINGVAR3 INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;

Example 2: Multiple Auto Parsers
================================

.. code-block:: xml

	STRINGVAR3 = '<PCARD_CARD_GET_DETAILResponse xmlns="http://tempuri.org/">
      <PCARD_CARD_GET_DETAILResult>
        <ConditionNo>100</ConditionNo>
        <ConditionMesaj>Invalid username or password</ConditionMesaj>
        <Basarili>false</Basarili>
        <cardInfo />
        <cardCredit>
          <lmt xsi:nil="true" />
          <dfl_amt xsi:nil="true" />
          <bln xsi:nil="true" />
          <dsc xsi:nil="true" />
        </cardCredit>
      </PCARD_CARD_GET_DETAILResult>
    </PCARD_CARD_GET_DETAILResponse>';

	PARSEXML TEXT STRINGVAR3 INTO TMPTABLE;
	PARSEXML TEXT TMPTABLE_XMLData INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;


Example 3: Using PCData
=======================

.. code-block:: xml

	<?xml version="1.0" ?>
	<CustomerList>
		<Customer>IAS Software</Customer>
		<Customer>XYZ Technology</Customer>
		<Customer>ABC Design</Customer>
	</CustomerList>

::

	OBJECT: 
		TABLE CUSTOMERS;

	CLEAR XMLMAP SALESMAP;
	CREATEXMLMAP SALESMAP;

	MAP ELEMENT CustomerList AS XMLROOTTABLE CustomerList IN SALESMAP;

	MAP ELEMENT Customer AS XMLTABLE Customer IN SALESMAP;
	MAP PCDATA OF Customer AS XMLCOLUMN Customer IN SALESMAP;

	MAP RELATION Customer TO CustomerList LINK 'asdf' WITH DUMMY  GENERATE NO IN SALESMAP;

	PARSEXML 'C:\TMP\tempxml2.xml' USING SALESMAP;
	CONVERTXMLTABLE Customer INTO TABLE CUSTOMERS FROM SALESMAP;

	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;


Example 4: Reading Childs
=========================

.. code-block:: xml

	<?xml version="1.0" ?>
	<CustomerList>
		<Sector>Technology</Sector>
		<Customer>
			<CustName>XYZ Industries</CustName>
			<City>Tokyo</City>
		</Customer>
		<Customer>
			<CustName>ABC Technology</CustName>
			<City>Madrid</City>
		</Customer>
		<Customer>
			<CustName>IAS Software</CustName>
			<City>Istanbul</City>
		</Customer>
	</CustomerList>

::

	OBJECT: 
		TABLE CUSTOMERS;

	CLEAR XMLMAP SALESMAP;
	CREATEXMLMAP SALESMAP;

	MAP ELEMENT CustomerList AS XMLROOTTABLE CustomerList IN SALESMAP;
	MAP CHILD Sector OF CustomerList AS XMLCOLUMN Sector IN SALESMAP;

	MAP ELEMENT Customer AS XMLTABLE CUSTOMER IN SALESMAP;
	MAP CHILD CustName OF Customer AS XMLCOLUMN CustName IN SALESMAP;
	MAP CHILD City OF Customer AS XMLCOLUMN City IN SALESMAP;

	MAP RELATION Customer TO CustomerList LINK Sector WITH Sector GENERATE YES IN SALESMAP;

	PARSEXML 'C:\TMP\tempxml2.xml' USING SALESMAP;
	CONVERTXMLTABLE CUSTOMER INTO TABLE CUSTOMERS FROM SALESMAP;

	STRINGVAR1 = CUSTOMERS_ROWCOUNT;

	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;


Example 5: Childs With Extra Relation
=====================================

.. code-block:: xml
	
	<PCARD_CARD_GET_DETAILResponse xmlns="http://tempuri.org/">
		  <PCARD_CARD_GET_DETAILResult>
			<ConditionNo>100</ConditionNo>
			<ConditionMesaj>Invalid username or password</ConditionMesaj>
			<Basarili>false</Basarili>
			<cardInfo />
			<cardCredit>
			  <lmt/>
			  <dfl_amt/>
			  <bln/>
			  <dsc/>
			</cardCredit>
		  </PCARD_CARD_GET_DETAILResult>
	</PCARD_CARD_GET_DETAILResponse>
	
::

	OBJECT: 
		TABLE DETAILRESULT;

	CLEAR XMLMAP SALESMAP;
	CREATEXMLMAP SALESMAP;

	MAP ELEMENT PCARD_CARD_GET_DETAILResponse AS XMLROOTTABLE PCARD_CARD_GET_DETAILResponse IN SALESMAP;

	MAP ELEMENT PCARD_CARD_GET_DETAILResult AS XMLTABLE PCARD_CARD_GET_DETAILResult IN SALESMAP;
	MAP CHILD ConditionNo OF PCARD_CARD_GET_DETAILResult AS XMLCOLUMN ConditionNo IN SALESMAP;
	MAP CHILD ConditionMesaj OF PCARD_CARD_GET_DETAILResult AS XMLCOLUMN ConditionMesaj IN SALESMAP;
	MAP CHILD Basarili OF PCARD_CARD_GET_DETAILResult AS XMLCOLUMN Basarili IN SALESMAP;
	MAP CHILD ConditionNo OF PCARD_CARD_GET_DETAILResult AS XMLCOLUMN ConditionNo IN SALESMAP;

	MAP RELATION PCARD_CARD_GET_DETAILResult TO PCARD_CARD_GET_DETAILResponse LINK DUMMY WITH DUMMY  GENERATE NO IN SALESMAP;

	PARSEXML 'C:\TMP\tempxml2.xml' USING SALESMAP;
	CONVERTXMLTABLE PCARD_CARD_GET_DETAILResult INTO TABLE DETAILRESULT FROM SALESMAP;

	COPY TABLE DETAILRESULT INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;


Example 6: Reading Attributes
=============================

.. code-block:: xml

	<?xml version="1.0" ?>
	<CustomerList Sector="Technology">
		<Customer>
			<CustName>XYZ Industries</CustName>
			<City>Tokyo</City>
		</Customer>
		<Customer>
			<CustName>ABC Technology</CustName>
			<City>Madrid</City>
		</Customer>
		<Customer>
			<CustName>IAS Software</CustName>
			<City>Istanbul</City>
		</Customer>
	</CustomerList>

::

	OBJECT: 
		TABLE CUSTOMERS;

	CLEAR XMLMAP SALESMAP;
	CREATEXMLMAP SALESMAP;

	MAP ELEMENT CustomerList AS XMLROOTTABLE CustomerList IN SALESMAP;

	/* map attribute for root element*/
	MAP ATTRIBUTE Sector OF CustomerList AS XMLCOLUMN Sector IN SALESMAP;

	MAP ELEMENT Customer AS XMLTABLE CUSTOMER IN SALESMAP;
	MAP CHILD CustName OF Customer AS XMLCOLUMN CustName IN SALESMAP;
	MAP CHILD City OF Customer AS XMLCOLUMN City IN SALESMAP;

	MAP RELATION Customer TO CustomerList LINK Sector WITH Sector GENERATE YES IN SALESMAP;

	PARSEXML 'C:\TMP\tempxml2.xml' USING SALESMAP;
	CONVERTXMLTABLE CUSTOMER INTO TABLE CUSTOMERS FROM SALESMAP;

	STRINGVAR1 = CUSTOMERS_ROWCOUNT;

	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;


Example 7: Attributes On Childs
===============================


.. code-block:: xml

	<?xml version="1.0" ?>
	<CustomerList Sector="Technology">
		<Customer CustomerId="C1">
			<CustName>XYZ Industries</CustName>
			<City>Tokyo</City>
		</Customer>
		<Customer CustomerId="C2">
			<CustName>ABC Technology</CustName>
			<City>Madrid</City>
		</Customer>
		<Customer  CustomerId="C3">
			<CustName>IAS Software</CustName>
			<City>Istanbul</City>
		</Customer>
	</CustomerList>


::

	OBJECT: 
		TABLE CUSTOMERS;

	CLEAR XMLMAP SALESMAP;
	CREATEXMLMAP SALESMAP;

	MAP ELEMENT CustomerList AS XMLROOTTABLE CustomerList IN SALESMAP;
	MAP ATTRIBUTE Sector OF CustomerList AS XMLCOLUMN Sector IN SALESMAP;

	MAP ELEMENT Customer AS XMLTABLE CUSTOMER IN SALESMAP;
	/* attribute for child */
	MAP ATTRIBUTE CustomerId OF Customer AS XMLCOLUMN CustomerId IN SALESMAP;
	MAP CHILD CustName OF Customer AS XMLCOLUMN CustName IN SALESMAP;
	MAP CHILD City OF Customer AS XMLCOLUMN City IN SALESMAP;

	MAP RELATION Customer TO CustomerList LINK Sector WITH Sector GENERATE YES IN SALESMAP;

	PARSEXML 'C:\TMP\tempxml2.xml' USING SALESMAP;
	CONVERTXMLTABLE CUSTOMER INTO TABLE CUSTOMERS FROM SALESMAP;

	STRINGVAR1 = CUSTOMERS_ROWCOUNT;

	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;

	


Example 8: Creating Multiple Tables and Relations Childs-Attributes
===================================================================

.. code-block:: xml

	<?xml version="1.0" ?>
	<CustomerList Sector="Technology">
		
		<Customer CustomerId="C1">
			<CustName>XYZ Industries</CustName>
			<City>Tokyo</City>
			<Order OrderId="O1">
				<Year>2013</Year>
				<GrandTotal>100.013</GrandTotal>
			</Order>
		</Customer>
		
		<Customer CustomerId="C2">
			<CustName>ABC Technology</CustName>
			<City>Madrid</City>

			<Order OrderId="O2">
				<Year>2011</Year>
				<GrandTotal>100.011</GrandTotal>
			</Order>
			<Order OrderId="O3">
				<Year>2012</Year>
				<GrandTotal>100.012</GrandTotal>
			</Order>

		</Customer>
		
		<Customer  CustomerId="C3">
			<CustName>IAS Software</CustName>
			<City>Istanbul</City>

			<Order OrderId="O5">
				<Year>2010</Year>
				<GrandTotal>100.010</GrandTotal>
			</Order>
			<Order OrderId="O5">
				<Year>2014</Year>
				<GrandTotal>100.014</GrandTotal>
			</Order>

		</Customer>
	</CustomerList>
	
::

	OBJECT: 
		TABLE CUSTOMERS,
		TABLE ORDERS;

	CLEAR XMLMAP SALESMAP;
	CREATEXMLMAP SALESMAP;

	MAP ELEMENT CustomerList AS XMLROOTTABLE CustomerList IN SALESMAP;
	MAP ATTRIBUTE Sector OF CustomerList AS XMLCOLUMN Sector IN SALESMAP;

	MAP ELEMENT Customer AS XMLTABLE CUSTOMER IN SALESMAP;
	MAP ATTRIBUTE CustomerId OF Customer AS XMLCOLUMN CustomerId IN SALESMAP;
	MAP CHILD CustName OF Customer AS XMLCOLUMN CustName IN SALESMAP;
	MAP CHILD City OF Customer AS XMLCOLUMN City IN SALESMAP;
	MAP RELATION Customer TO CustomerList LINK Sector WITH Sector GENERATE YES IN SALESMAP;

	MAP ELEMENT Order AS XMLTABLE ORDER IN SALESMAP;
	MAP ATTRIBUTE OrderId OF Order AS XMLCOLUMN OrderId IN SALESMAP;
	MAP CHILD Year OF Order AS XMLCOLUMN Year IN SALESMAP;
	MAP CHILD GrandTotal OF Order AS XMLCOLUMN GrandTotal IN SALESMAP;
	MAP RELATION Order TO Customer LINK CustomerId WITH CustomerId GENERATE NO IN SALESMAP;

	PARSEXML 'C:\TMP\tempxml2.xml' USING SALESMAP;
	CONVERTXMLTABLE CUSTOMER INTO TABLE CUSTOMERS FROM SALESMAP;
	CONVERTXMLTABLE ORDER INTO TABLE ORDERS FROM SALESMAP;

	STRINGVAR1 = CUSTOMERS_ROWCOUNT;

	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;

	COPY TABLE ORDERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;


Example 9: Multiple Relations - Extra OrdersColumnWithoutOrdersId
=================================================================

.. code-block:: xml

	<?xml version="1.0" ?>
	<CustomerList Sector="Technology">
		
		<Customer CustomerId="C1">
			<CustName>XYZ Industries</CustName>
			<City>Tokyo</City>
			<Orders>
				<Order OrderId="O1">
					<Year>2013</Year>
					<GrandTotal>100.013</GrandTotal>
				</Order>
			</Orders>
		</Customer>
		
		<Customer CustomerId="C2">
			<CustName>ABC Technology</CustName>
			<City>Madrid</City>
			<Orders>
				<Order OrderId="O2">
					<Year>2011</Year>
					<GrandTotal>100.011</GrandTotal>
				</Order>
				<Order OrderId="O3">
					<Year>2012</Year>
					<GrandTotal>100.012</GrandTotal>
				</Order>
			</Orders>
		</Customer>
		
		<Customer  CustomerId="C3">
			<CustName>IAS Software</CustName>
			<City>Istanbul</City>
			<Orders>
				<Order OrderId="O4">
					<Year>2010</Year>
					<GrandTotal>100.010</GrandTotal>
				</Order>
				<Order OrderId="O5">
					<Year>2014</Year>
					<GrandTotal>100.014</GrandTotal>
				</Order>
				<Order OrderId="O6">
					<Year>2015</Year>
					<GrandTotal>100.015</GrandTotal>
				</Order>
			</Orders>
		</Customer>
	</CustomerList>
	
::
	
	OBJECT: 
		TABLE CUSTOMERS,
		TABLE ALLORDERS,
		TABLE CUSTOMERORDERS;

	CLEAR XMLMAP SALESMAP;
	CREATEXMLMAP SALESMAP;

	MAP ELEMENT CustomerList AS XMLROOTTABLE CustomerList IN SALESMAP;
	MAP ATTRIBUTE Sector OF CustomerList AS XMLCOLUMN Sector IN SALESMAP;

	MAP ELEMENT Customer AS XMLTABLE CUSTOMER IN SALESMAP;
	MAP ATTRIBUTE CustomerId OF Customer AS XMLCOLUMN CustomerId IN SALESMAP;
	MAP CHILD CustName OF Customer AS XMLCOLUMN CustName IN SALESMAP;
	MAP CHILD City OF Customer AS XMLCOLUMN City IN SALESMAP;

	MAP RELATION Customer TO CustomerList LINK Sector WITH Sector GENERATE YES IN SALESMAP;

	MAP ELEMENT Orders AS XMLTABLE CUSTOMERORDER IN SALESMAP;
	MAP RELATION Orders TO Customer LINK CustomerId WITH CustomerId GENERATE YES IN SALESMAP;

	MAP ELEMENT Order AS XMLTABLE ORDER IN SALESMAP;
	MAP ATTRIBUTE OrderId OF Order AS XMLCOLUMN OrderId IN SALESMAP;
	MAP CHILD Year OF Order AS XMLCOLUMN Year IN SALESMAP;
	MAP CHILD GrandTotal OF Order AS XMLCOLUMN GrandTotal IN SALESMAP;
	MAP RELATION Order TO Orders LINK CustomerFK WITH CustomerFK GENERATE YES IN SALESMAP;

	PARSEXML 'C:\TMP\tempxml2.xml' USING SALESMAP;
	CONVERTXMLTABLE CUSTOMER INTO TABLE CUSTOMERS FROM SALESMAP;
	CONVERTXMLTABLE CUSTOMERORDER INTO TABLE CUSTOMERORDERS FROM SALESMAP;
	CONVERTXMLTABLE ORDER INTO TABLE ALLORDERS FROM SALESMAP;

	STRINGVAR1 = CUSTOMERS_ROWCOUNT;

	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;

	COPY TABLE ALLORDERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;

	/*
	LOOP AT ALLORDERS
	BEGIN
		LOCATERECORD BINARYSEARCH COLUMNS CustomerFK VALUES ALLORDERS_CustomerFK ON CUSTOMERORDERS;
		STRINGVAR2 = STRINGVAR2 + ' ' + CUSTOMERORDERS_CustomerId;
		ALLORDERS_CustomerFK = CUSTOMERORDERS_CustomerId;
	ENDLOOP;
	*/

	COPY TABLE ALLORDERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;
	RETURN;
	COPY TABLE CUSTOMERORDERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;


Example 10:
===========


Example 11:
===========


Example 12:
===========






Exercise 1: Parse using READXMLSTRUCTURE
----------------------------------------

Exercise 2: Parsing Simple XML Documents
----------------------------------------

Exercise 3: Parsing Complex XML Documents
-----------------------------------------

Exercise 4: Validation
----------------------







	