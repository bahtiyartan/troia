=============================
Working With LLM Models
=============================

*A vector database is a type of database that stores data as mathematical “vectors” rather than using the traditional row/column structure, and is designed to perform similarity searches. It is especially used in the field of artificial intelligence. This section aims to explain how vector database operations can be performed with TROIA.*


Header?
----------------------------

**A vector database is a specialized type of database that stores data in vector (embedded) format and performs vector similarity calculations on these vectors to find the closest results.** Vector DB performs searches using numerical vectors, and simultaneously stores the text associated with this numerical data. Thus, after converting a given text into a numerical vector, we can obtain clear, human- and LLM-readable versions of the resulting text.


Subheader?
====================

test

::

	OBJECT:
		STRING CONNAME,
		STRING EID;

	CONNAME = 'MyConnectionName';
	EID = 'DEVQDRANT';


	MAKEENDPOINTCONNECTION CONNAME ENDPOINTID EID;

	IF SYS_STATUS == 0 THEN

		//do your endpoint actions here

		CLOSEENDPOINTCONNECTION CONNAME;

		IF SYS_STATUS == 1 THEN
			STRINGVAR3 = SYS_STATUS + ' ' + SYS_STATUSERROR;
		ENDIF;
	ELSE
		STRINGVAR3 = SYS_STATUS + ' ' + SYS_STATUSERROR;
	ENDIF;


In SYST51, endpoint type of the configuration must be a vector db product. Actual supported vector databses are listed on SYST51 - Integration Endpoints Configuration.

Deleting Collections
====================

To delete an existing collection, the action name must be DELETECOLLECTION. Here is the sample command.
Of course you must have a db connection before running VECTORDBACTION command like the examples above.

::

	VECTORDBACTION DELETECOLLECTION CONNECTIONNAME CONNAME COLLECTIONNAME COLNAME;

::

	OBJECT:
		STRING CONNAME,
		STRING EID,
		STRING COLNAME,
		STRING COLPARAMS,
		STRING MYERROR;

	CONNAME = 'MyQdrant';
	EID = 'DEVQDRANT';
	COLNAME = 'testcollection';
	COLPARAMS = '{
		"vectors": {
			"size": 4,
			"distance": "Cosine",
			"on_disk": true
		},
		"hnsw_config": {
			"m": 16,
			"ef_construct": 100
		},
		"optimizer_config": {
			"indexing_threshold": 100
		}
	}';

	MAKEENDPOINTCONNECTION CONNAME ENDPOINTID EID;

	IF SYS_STATUS == 0 THEN
		VECTORDBACTION
			CREATECOLLECTION CONNECTIONNAME CONNAME COLLECTIONNAME COLNAME
			COLLECTIONPARAMS COLPARAMS;

		IF SYS_STATUS == 1 THEN
			MYERROR = SYS_STATUS + ' ' + SYS_STATUSERROR;
		ENDIF;

		CLOSEENDPOINTCONNECTION CONNAME;
	ENDIF;

For more and all supported opeations about collections please see TROIA help.
