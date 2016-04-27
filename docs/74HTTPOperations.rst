

======================================
HTTP Operations & Calling Web Services
======================================

*HTTP (Hypertext Transfer Protocol) is a widely used application protocol for distributed, collaborative information systems. Also web services is another widely used solution to collaborate heterogeneous systems over the web. This sections aims to introduce HTTP operations and calling web services in TROIA*


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





	