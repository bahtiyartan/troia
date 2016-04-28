

======================================
HTTP Operations & Calling Web Services
======================================

*HTTP (Hypertext Transfer Protocol) is a widely used application protocol for distributed, collaborative information systems. Also web services is another widely used solution to collaborate heterogeneous systems over the web. This section aims to introduce HTTP operations and calling web services in TROIA*


Sending data over HTTP Post Method
----------------------------------

::

	SENDHTTPPOST {poststr} TO {url} [CODEPAGE {encoding}] [CONTENTTYPE {contenttype}]
				[COOKIE {cookie}] [REFERER {referer}] 
				[HEADERS {headers}] [REQUESTMETHOD {requestmethod}]; 
				
poststring, url, encoding, contenttype, cookie,headers,requestmethod


Reading HTTP Response
=====================

SYS_SENDHTTPPOSTRESPONSE

SYS_HTTPPOSTCOOKIES

::

	OBJECT: 
		STRING STRINGVAR3,
		STRING STRINGVAR2,
		STRING STRURL,
		STRING FILEPATH;

	FILEPATH = '*C:\TMP\response.html';
	STRURL = 'http://troia.readthedocs.org/en/latest/search.html';

	SENDHTTPPOST '' TO STRURL;
	STRINGVAR2 = SYS_HTTPPOSTCOOKIES;
	STRINGVAR3 = SYS_HTTPPOSTRESPONSE;

	OPEN FILE FILEPATH FORNEW;
	PUT STRINGVAR3, CODEPAGE 'UTF-8';
	CLOSE FILE;
	
::

	OBJECT: 
		STRING STRINGVAR3,
		STRING STRINGVAR2,
		STRING STRURL,
		STRING FILEPATH,
		STRING POSTPARAM;

	STRURL = 'http://192.168.0.2:8080/FileUploader/index.jsp';
	SENDHTTPPOST '' TO STRURL;

	STRINGVAR2 = SYS_HTTPPOSTCOOKIES;
	STRINGVAR3 = SYS_HTTPPOSTRESPONSE;

	POSTPARAM = 'username=btan&password=xxxx';

	STRURL = 'http://192.168.0.2:8080/FileUploader/login';
	SENDHTTPPOST POSTPARAM TO STRURL COOKIE STRINGVAR2;

	STRINGVAR3 = SYS_HTTPPOSTRESPONSE;

	FILEPATH = '*C:\TMP\response.html';
	OPEN FILE FILEPATH FORNEW;
	PUT STRINGVAR3, CODEPAGE 'UTF-8';
	CLOSE FILE;




Calling Web Services
--------------------

::

	 CALLWEBSERVICE {wsdlurl} , {operation} TO {sym} WITH {params};

::

	OBJECT: 
		STRING STRWSDL,
		STRING RESULT,
		TABLE TMPTABLE,
		STRING STRINGVAR3;

	STRWSDL = 'http://footballpool.dataaccess.eu/data/info.wso?wsdl';
	CALLWEBSERVICE STRWSDL, 'TopGoalScorers' TO RESULT WITH 25;

	INTEGERVAR1 = STRPOS(RESULT, '<m:TopGoalScorersResult>')-1;
	INTEGERVAR2 = STRPOS(RESULT, '</m:TopGoalScorersResult>') - INTEGERVAR1 -1;
	RESULT = STRSTR(RESULT,INTEGERVAR1,INTEGERVAR2) + '</m:TopGoalScorersResult>';
	RESULT = REPLACE(RESULT,'m:','');
	STRINGVAR3 = RESULT;

	PARSEXML TEXT RESULT INTO TMPTABLE;
	REMOVE COLUMN tTopGoalScorersCountry PERMANENT FROM TMPTABLE;
	REMOVE COLUMN tTopGoalScorersFlag PERMANENT FROM TMPTABLE;
	REMOVE COLUMN tTopGoalScorersFlagLarge PERMANENT FROM TMPTABLE;

	SET TMPTABLE TO TABLE TMPTABLE;

::

	OBJECT: 
		STRING STRWSDL,
		STRING RESULT,
		TABLE TMPTABLE,
		STRING STRINGVAR3;

	STRWSDL = 'http://www.webservicex.net/globalweather.asmx?WSDL';
	CALLWEBSERVICE STRWSDL, 'GetCitiesByCountry' TO RESULT WITH 'Germany';
	STRINGVAR3 = RESULT;

	PARSEXML TEXT RESULT INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;
	
::

	OBJECT: 
		STRING STRWSDL,
		STRING RESULT,
		TABLE TMPTABLE,
		STRING STRINGVAR3;

	STRWSDL = 'http://www.w3schools.com/xml/tempconvert.asmx?WSDL';
	CALLWEBSERVICE STRWSDL, 'CelsiusToFahrenheit' TO RESULT WITH '10';
	STRINGVAR3 = RESULT;
	
::

	OBJECT: 
		STRING STRINGVAR3,
		STRING STRURL,
		STRING FILEPATH,
		STRING STRCTYPE,
		TABLE TMPTABLE;

	FILEPATH = '*C:\TMP\response.html';
	STRURL = 'http://www.w3schools.com/xml/tempconvert.asmx/CelsiusToFahrenheit';
	STRCTYPE = 'application/x-www-form-urlencoded';

	SENDHTTPPOST 'celsius=10' TO STRURL CONTENTTYPE STRCTYPE;
	STRINGVAR3 = SYS_HTTPPOSTRESPONSE;

	PARSEXML TEXT STRINGVAR3 INTO TMPTABLE;
	SET TMPTABLE TO TABLE TMPTABLE;



	