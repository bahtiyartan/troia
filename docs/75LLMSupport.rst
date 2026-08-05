=============================
Working With LLM Models
=============================

*The LLM (Large Language Model) is an artificial intelligence model trained on very large amounts of text and capable of understanding and generating natural language. This section aims to explain how to connect to an AI model via the TROIA Platform.*


What is an Large Language Model?
--------------------------------

...


How to interact with an LLM Model?
==================================


LLM Gateway
---------------------------


How to configure LLMGateway
============================


How to Define an LLMGateway as an Endpoint
==========================================


Interaction Methods with LLM Gateway
------------------------------------


LLM Prompt on Clients
=====================


Accessing LLM Gateway Programmatically
=======================================

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
