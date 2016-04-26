

==================
Parsing XML & JSON
==================

*Both XML and JSON are common formats to transfer data between systems in a human readable format. This sections aims to introduce all options to handle/parse xml & json formats.*


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

..xmlparsing

Example 1: Using Auto Parser
----------------------------

.. code-block:: xml
	
	OBJECT:
		STRING STRINGVAR3;

	STRINGVAR3 = '<?xml version="1.0" ?>
	<CustomerList>
			<Customer>
				<Name>IAS Software</Name>
				<Country>Turkey</Country>
			</Customer>
			<Customer>
				<Name>XYZ Technology</Name>
				<Country>Turkey</Country>
			</Customer>
			<Customer>
				<Name>ABC Design</Name>
				<Country>Turkey</Country>
			</Customer>
	</CustomerList>';

	OBJECT: 
		TABLE CUSTOMERS;

	PARSEXML TEXT STRINGVAR3 INTO CUSTOMERS;

	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;

Example 2: Multiple Auto Parsers
--------------------------------

.. code-block:: xml

	OBJECT:
		STRING STRINGVAR3;

	STRINGVAR3 = '<PCARD_CARD_GET_DETAILResponse xmlns="http://tempuri.org/">
		<PCARD_CARD_GET_DETAILResult>
			<ConditionNo>100</ConditionNo>
			<ConditionMessage>Invalid username or password</ConditionMessage>
			<Success>false</Success>
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
-----------------------

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

	CLEAR XMLMAP DEMOMAP;
	CREATEXMLMAP DEMOMAP;
	
	MAP ELEMENT CustomerList AS XMLROOTTABLE CustomerList IN DEMOMAP;
	MAP ELEMENT Customer AS XMLTABLE Customer IN DEMOMAP;
	MAP PCDATA OF Customer AS XMLCOLUMN Customer IN DEMOMAP;
	MAP RELATION Customer TO CustomerList LINK 'asdf' 
						WITH DUMMY  GENERATE NO IN DEMOMAP;
	
	COPYFILE '*C:\TMP\XML\ex3.xml' INTO '.\XMLParsing\ex3.xml';
	PARSEXML '.\XMLParsing\ex3.xml' USING DEMOMAP;
	
	CONVERTXMLTABLE Customer INTO TABLE CUSTOMERS FROM DEMOMAP;
	
	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;


Example 4: Reading Childs
-------------------------

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
		TABLE CUSTOMERS,
		INTEGER ROWCOUNT;

	CLEAR XMLMAP DEMOMAP;
	CREATEXMLMAP DEMOMAP;
	MAP ELEMENT CustomerList AS XMLROOTTABLE CustomerList IN DEMOMAP;
	MAP CHILD Sector OF CustomerList AS XMLCOLUMN Sector IN DEMOMAP;
	MAP ELEMENT Customer AS XMLTABLE CUSTOMER IN DEMOMAP;
	MAP CHILD CustName OF Customer AS XMLCOLUMN CustName IN DEMOMAP;
	MAP CHILD City OF Customer AS XMLCOLUMN City IN DEMOMAP;
	MAP RELATION Customer TO CustomerList LINK Sector 
						WITH Sector GENERATE YES IN DEMOMAP;

	COPYFILE '*C:\TMP\XML\ex4.xml' INTO '.\XMLParsing\ex4.xml';
	PARSEXML '.\XMLParsing\ex4.xml' USING DEMOMAP;
	CONVERTXMLTABLE CUSTOMER INTO TABLE CUSTOMERS FROM DEMOMAP;

	ROWCOUNT = CUSTOMERS_ROWCOUNT;
	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;



Example 5: Childs with Extra Relation
-------------------------------------

.. code-block:: xml
	
	<PCARD_CARD_GET_DETAILResponse xmlns="http://tempuri.org/">
		  <PCARD_CARD_GET_DETAILResult>
			<ConditionNo>100</ConditionNo>
			<ConditionMessage>Invalid username or password</ConditionMessage>
			<Success>false</Success>
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

	CLEAR XMLMAP DEMOMAP;
	CREATEXMLMAP DEMOMAP;
	
	MAP ELEMENT PCARD_CARD_GET_DETAILResponse 
			AS XMLROOTTABLE PCARD_CARD_GET_DETAILResponse IN DEMOMAP;
	MAP ELEMENT PCARD_CARD_GET_DETAILResult 
				AS XMLTABLE PCARD_CARD_GET_DETAILResult IN DEMOMAP;
	MAP CHILD ConditionNo OF PCARD_CARD_GET_DETAILResult 
					AS XMLCOLUMN ConditionNo IN DEMOMAP;
	MAP CHILD ConditionMessage OF PCARD_CARD_GET_DETAILResult 
					AS XMLCOLUMN ConditionMesaj IN DEMOMAP;
	MAP CHILD Success OF PCARD_CARD_GET_DETAILResult 
					AS XMLCOLUMN Success IN DEMOMAP;
	MAP CHILD ConditionNo OF PCARD_CARD_GET_DETAILResult 
					AS XMLCOLUMN ConditionNo IN DEMOMAP;
	
	MAP RELATION PCARD_CARD_GET_DETAILResult TO PCARD_CARD_GET_DETAILResponse LINK
					DUMMY WITH DUMMY  GENERATE NO IN DEMOMAP;
	
	COPYFILE '*C:\TMP\XML\ex5.xml' INTO '.\XMLParsing\ex5.xml';
	PARSEXML '.\XMLParsing\ex5.xml' USING DEMOMAP;
	CONVERTXMLTABLE PCARD_CARD_GET_DETAILResult 
						INTO TABLE DETAILRESULT FROM DEMOMAP;

	COPY TABLE DETAILRESULT INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;


Example 6: Reading Attributes
-----------------------------

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
		TABLE CUSTOMERS,
		INTEGER ROWCOUNT;

	CLEAR XMLMAP DEMOMAP;
	CREATEXMLMAP DEMOMAP;

	MAP ELEMENT CustomerList AS XMLROOTTABLE CustomerList IN DEMOMAP;

	/* map attribute for root element*/
	MAP ATTRIBUTE Sector OF CustomerList AS XMLCOLUMN Sector IN DEMOMAP;

	MAP ELEMENT Customer AS XMLTABLE CUSTOMER IN DEMOMAP;
	MAP CHILD CustName OF Customer AS XMLCOLUMN CustName IN DEMOMAP;
	MAP CHILD City OF Customer AS XMLCOLUMN City IN DEMOMAP;

	MAP RELATION Customer TO CustomerList 
			LINK Sector WITH Sector GENERATE YES IN DEMOMAP;

	COPYFILE '*C:\TMP\XML\ex6.xml' INTO '.\XMLParsing\ex6.xml';
    PARSEXML '.\XMLParsing\ex6.xml' USING DEMOMAP;
	CONVERTXMLTABLE CUSTOMER INTO TABLE CUSTOMERS FROM DEMOMAP;

	ROWCOUNT = CUSTOMERS_ROWCOUNT;
	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;


Example 7: Attributes On Childs
-------------------------------


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
		TABLE CUSTOMERS,
		INTEGER ROWCOUNT;

	CLEAR XMLMAP DEMOMAP;
	CREATEXMLMAP DEMOMAP;

	MAP ELEMENT CustomerList AS XMLROOTTABLE CustomerList IN DEMOMAP;
	MAP ATTRIBUTE Sector OF CustomerList AS XMLCOLUMN Sector IN DEMOMAP;

	MAP ELEMENT Customer AS XMLTABLE CUSTOMER IN DEMOMAP;
	/* attribute for child */
	MAP ATTRIBUTE CustomerId OF Customer AS XMLCOLUMN CustomerId IN DEMOMAP;
	MAP CHILD CustName OF Customer AS XMLCOLUMN CustName IN DEMOMAP;
	MAP CHILD City OF Customer AS XMLCOLUMN City IN DEMOMAP;

	MAP RELATION Customer TO CustomerList LINK Sector
						WITH Sector GENERATE YES IN DEMOMAP;

	COPYFILE '*C:\TMP\XML\ex7.xml' INTO '.\XMLParsing\ex7.xml';
	PARSEXML '.\XMLParsing\ex7.xml' USING DEMOMAP;
	CONVERTXMLTABLE CUSTOMER INTO TABLE CUSTOMERS FROM DEMOMAP;

	ROWCOUNT = CUSTOMERS_ROWCOUNT;

	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;

	


Example 8: Multiple Tables & Relations Childs-Attributes
--------------------------------------------------------

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
			<Order OrderId="O6">
				<Year>2014</Year>
				<GrandTotal>100.014</GrandTotal>
			</Order>

		</Customer>
	</CustomerList>
	
::

	OBJECT: 
		TABLE CUSTOMERS,
		TABLE ORDERS,
		INTEGER ROWCOUNT;

	CLEAR XMLMAP DEMOMAP;
	CREATEXMLMAP DEMOMAP;

	MAP ELEMENT CustomerList AS XMLROOTTABLE CustomerList IN DEMOMAP;
	MAP ATTRIBUTE Sector OF CustomerList AS XMLCOLUMN Sector IN DEMOMAP;

	MAP ELEMENT Customer AS XMLTABLE CUSTOMER IN DEMOMAP;
	MAP ATTRIBUTE CustomerId OF Customer AS XMLCOLUMN CustomerId IN DEMOMAP;
	MAP CHILD CustName OF Customer AS XMLCOLUMN CustName IN DEMOMAP;
	MAP CHILD City OF Customer AS XMLCOLUMN City IN DEMOMAP;
	MAP RELATION Customer TO CustomerList LINK Sector 
					WITH Sector GENERATE YES IN DEMOMAP;

	MAP ELEMENT Order AS XMLTABLE ORDER IN DEMOMAP;
	MAP ATTRIBUTE OrderId OF Order AS XMLCOLUMN OrderId IN DEMOMAP;
	MAP CHILD Year OF Order AS XMLCOLUMN Year IN DEMOMAP;
	MAP CHILD GrandTotal OF Order AS XMLCOLUMN GrandTotal IN DEMOMAP;
	MAP RELATION Order TO Customer LINK CustomerId
					WITH CustomerId GENERATE NO IN DEMOMAP;

	COPYFILE '*C:\TMP\XML\ex8.xml' INTO '.\XMLParsing\ex8.xml';
	PARSEXML '.\XMLParsing\ex8.xml' USING DEMOMAP;
	CONVERTXMLTABLE CUSTOMER INTO TABLE CUSTOMERS FROM DEMOMAP;
	CONVERTXMLTABLE ORDER INTO TABLE ORDERS FROM DEMOMAP;

	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;
	RETURN;
	COPY TABLE ORDERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;


Example 9: Multiple Relations/Extra Orders Column Without OrdersId
------------------------------------------------------------------

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

	CLEAR XMLMAP DEMOMAP;
	CREATEXMLMAP DEMOMAP;

	MAP ELEMENT CustomerList AS XMLROOTTABLE CustomerList IN DEMOMAP;
	MAP ATTRIBUTE Sector OF CustomerList AS XMLCOLUMN Sector IN DEMOMAP;

	MAP ELEMENT Customer AS XMLTABLE CUSTOMER IN DEMOMAP;
	MAP ATTRIBUTE CustomerId OF Customer AS XMLCOLUMN CustomerId IN DEMOMAP;
	MAP CHILD CustName OF Customer AS XMLCOLUMN CustName IN DEMOMAP;
	MAP CHILD City OF Customer AS XMLCOLUMN City IN DEMOMAP;

	MAP RELATION Customer TO CustomerList LINK Sector 
						WITH Sector GENERATE YES IN DEMOMAP;

	MAP ELEMENT Orders AS XMLTABLE CUSTOMERORDER IN DEMOMAP;
	MAP RELATION Orders TO Customer LINK CustomerId 
						WITH CustomerId GENERATE YES IN DEMOMAP;

	MAP ELEMENT Order AS XMLTABLE ORDER IN DEMOMAP;
	MAP ATTRIBUTE OrderId OF Order AS XMLCOLUMN OrderId IN DEMOMAP;
	MAP CHILD Year OF Order AS XMLCOLUMN Year IN DEMOMAP;
	MAP CHILD GrandTotal OF Order AS XMLCOLUMN GrandTotal IN DEMOMAP;
	MAP RELATION Order TO Orders LINK CustomerFK 
						WITH CustomerFK GENERATE YES IN DEMOMAP;

	COPYFILE '*C:\TMP\XML\ex9.xml' INTO '.\XMLParsing\ex9.xml';
	PARSEXML '.\XMLParsing\ex9.xml' USING DEMOMAP;
	CONVERTXMLTABLE CUSTOMER INTO TABLE CUSTOMERS FROM DEMOMAP;
	CONVERTXMLTABLE CUSTOMERORDER INTO TABLE CUSTOMERORDERS FROM DEMOMAP;
	CONVERTXMLTABLE ORDER INTO TABLE ALLORDERS FROM DEMOMAP;

	COPY TABLE CUSTOMERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;

	COPY TABLE ALLORDERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;

	COPY TABLE CUSTOMERORDER INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;

	LOOP AT ALLORDERS
	BEGIN
		LOCATERECORD BINARYSEARCH COLUMNS CustomerFK VALUES 
						ALLORDERS_CustomerFK ON CUSTOMERORDERS;
		STRINGVAR2 = STRINGVAR2 + ' ' + CUSTOMERORDERS_CustomerId;
		ALLORDERS_CustomerFK = CUSTOMERORDERS_CustomerId;
	ENDLOOP;


	COPY TABLE ALLORDERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;

	COPY TABLE CUSTOMERORDERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;
	
	
Example 10: Relation with Non-Generated Column
----------------------------------------------

.. code-block:: xml

	<?xml version="1.0" encoding="UTF-8"?>
	<string xmlns="http://tempuri.org/">
	  <Orders>
		<Order id="ORDER1">
		  <Order_org>ORDERORG1</Order_org>
		  <Orderline id="ORDERLINE1">
			<OrderLine_org>ORDERORG1</OrderLine_org>
		  </Orderline>
		  <Orderline id="ORDERLINE2">
			<OrderLine_org>ORDERORG1</OrderLine_org>
		  </Orderline>
		</Order>
	  </Orders>
	</string>
	
::

	OBJECT: 
		TABLE ORDERLINES,
		TABLE ORDERS;

	CLEAR XMLMAP DEMOMAP;
	CREATEXMLMAP DEMOMAP;
	
	MAP ELEMENT string AS XMLROOTTABLE string IN DEMOMAP;
	MAP ATTRIBUTE xmlns OF string AS XMLCOLUMN xmlns IN DEMOMAP;
	
	MAP ELEMENT Orders AS XMLTABLE ALLORDERS IN DEMOMAP;
	MAP RELATION Orders TO string LINK xmlns WITH xmlns GENERATE NO IN DEMOMAP;
	
	MAP ELEMENT Order AS XMLTABLE CUSTOMERORDER IN DEMOMAP;
	MAP ATTRIBUTE id OF Order AS XMLCOLUMN orderid IN DEMOMAP;
	MAP CHILD Order_org OF Order AS XMLCOLUMN Order_org IN DEMOMAP;
	MAP RELATION Order TO Orders LINK CustomerId WITH DUMMY GENERATE NO IN DEMOMAP;
	
	MAP ELEMENT Orderline AS XMLTABLE ORDERLINE IN DEMOMAP;
	MAP ATTRIBUTE id OF Orderline AS XMLCOLUMN orderlineid IN DEMOMAP;
	MAP CHILD OrderLine_org OF Orderline AS XMLCOLUMN OrderLine_org IN DEMOMAP;
	MAP RELATION Orderline TO Order LINK orderid 
						WITH orderid GENERATE YES IN DEMOMAP;
	
	COPYFILE '*C:\TMP\XML\ex10.xml' INTO '.\XMLParsing\ex10.xml';
	PARSEXML '.\XMLParsing\ex10.xml' USING DEMOMAP;
	
	CONVERTXMLTABLE CUSTOMERORDER INTO TABLE ORDERS FROM DEMOMAP;
	CONVERTXMLTABLE ORDERLINE INTO TABLE ORDERLINES FROM DEMOMAP;
	CONVERTXMLTABLE ALLORDERS INTO TABLE ALLORDER FROM DEMOMAP;
	
	COPY TABLE ORDERLINES INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;
	
	COPY TABLE ORDERS INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;



Exercise 1: Parse using READXMLSTRUCTURE
----------------------------------------

Calculate sum of GrandTotal values of all orders listed in xml below, using READXMLSTRUCTURE command.

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
			<Order OrderId="O6">
				<Year>2014</Year>
				<GrandTotal>100.014</GrandTotal>
			</Order>
		</Customer>
	</CustomerList>

Exercise 2: Parsing Simple XML Documents
----------------------------------------

Calculate grand total value (sum of Price*Quantity for each product) for the products listed in XML document below:

.. code-block:: xml

	<ProductList>
		<Product>
			<Name>Product 1</Name>
			<Price>6.30</Price>
			<Quantity>5</Quantity>
        </Product>
		<Product>
			<Name>Product 2</Name>
			<Price>2.50</Price>
			<Quantity>3</Quantity>
		</Product>
		<Product>
			<Name>Product 3</Name>
			<Price>1.20</Price>
			<Quantity>2</Quantity>
		</Product>
	</ProductList>


#Exercise 3: Parsing Complex XML Documents
#-----------------------------------------

# Exercise 4: Validation
# ----------------------







	