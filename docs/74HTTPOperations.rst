

======================================
HTTP Operations & Calling Web Services
======================================

*HTTP (Hypertext Transfer Protocol) is a widely used application protocol for distributed, collaborative information systems. Also web services is another widely used solution to collaborate heterogeneous systems over the web. This sections aims to introduce HTTP operations and calling web services in TROIA*


Sending data over HTTP Post Method
----------------------------------

::

	SENDHTTPPOST {poststring} TO {url} [CODEPAGE {codepage}] [CONTENTTYPE {contenttype}]
				[COOKIE {cookie}] [REFERER {referer}] 
				[HEADERS {headers}] [REQUESTMETHOD {requestmethod}]; 


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



Calling Web Services
--------------------





	