=============================
Working With Vector Databases
=============================

*A vector database is a type of database that stores data as mathematical “vectors” rather than using the traditional row/column structure, and is designed to perform similarity searches. It is especially used in the field of artificial intelligence. This section aims to explain how vector database operations can be performed with TROIA.*


Vector Similarity and Similarity Search
---------------------------------------

**"Vector similarity" is the process of mathematically measuring how similar two vectors are to each other.** The most common methods for calculating vector similarity are Euclidean distance, cosine similarity, dot product etc. While "cosine similarity" is commonly used in RAG and LLM applications, the calculation methods used may vary depending on the requirements. However, to understand the approach, you can think of it using the Euclidean distance method, which is used to calculate the distance between two simple points.

After understanding the concept of "Vector Similarity," let's imagine we convert any text into a sequence of numbers and compare different sequences to calculate their proximity. In this case, we can assume that the texts represented by two vectors that are close to each other are also close to each other.

By setting a threshold value for this proximity comparison and only displaying results that exceed that threshold, we are essentially performing a "similarity search".

It is clear that we should use the same approach when converting the texts we are working with for similarity searching into a new number vector.


What is a "Vector Database"?
----------------------------

**A vector database is a specialized type of database that stores data in vector (embedded) format and performs vector similarity calculations on these vectors to find the closest results.** Vector DB performs searches using numerical vectors, and simultaneously stores the text associated with this numerical data. Thus, after converting a given text into a numerical vector, we can obtain clear, human- and LLM-readable versions of the resulting text.

**Vector databases are used in RAG applications because of these capabilities. RAG (Retrieval-Augmented Generation) is a technique that enables large language models (LLMs) to produce more accurate and up-to-date answers by bringing data from external information sources in addition to their own training data.** RAG is an artificial intelligence architecture that retrieves relevant information from a data source (Retrieval) before answering a user question, and then uses this information to generate an answer (Generation).

Like relational databases, vector databases are offered by many vendors targeting different markets, with varying licensing fees and data scale. Qdrant, Milvus, Weaviate Pinecone, and Chroma are among the most commonly used vector databases.


What is "Embedding"?
====================

**An "embedding" is a vector** that represents the meaning of data within a mathematical coordinate system. Embedding is the representation of text, images, or other data as a high-dimensional digital vector that preserves its semantic properties. These vectors form the basis of similarity calculations and semantic search operations.

The numbers in this embeddings themselves are not meaningful to humans. What matters is that the embeddings of semantically similar texts are close to each other. 

To write text to a vector database, that text must first be converted into a digital vector. In other words, we need embeddings. So the next question is "How can we obtain the embedding of a text, in other words, its vector containing numerical values?"

What is "Embedding Model"?
==========================

To obtain text embedding data, we need an "Embedding Model". **An Embedding Model is an artificial intelligence model** that takes text, an image, or other data and transforms it into an embedding (vector). 

As mentioned above, if we want to perform a "similarity search" on a vector database, we must write our data to the vector database using the same "Embedding Model" as the text we will use in the search. Writing data with embedding values ​​obtained using different methods to any collection in your vector database is not a correct approach.

We will discuss the components and system tools needed to obtain text embedding values ​​via the TROIA Platform in later sections of this chapter. For now, make sure you have a correct understanding of these basic concepts.


What is Semantic Search?
===========================

Bringing all these concepts together, Semantic Search is a search method that creates embeddings of user queries and content, calculates vector similarity between them, and finds the most relevant results in terms of meaning. In other words, **semantic search is a similarity search application performed on embeddings that are designed to represent meaning.** This approach forms the basis of RAG (Retrieval-Augmented Generation) applications.


Managing Vector DB Connections
------------------------------

To access a vector database via TROIA and perform any operation, a connection must first be established. Once the connection is established, it is possible to create new collections, add or delete data from existing collections in vector databases. After the connection is established and the operation is performed, the connection must be closed. So managing vector db connection is on TROIA developer's' responsibility.

To establish a connection, first you must have an endpoint definition. To learn more about endpoints please see the section "Integration Endpoints" of this book. 

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


Managing Collections
--------------------

Before querying a collection or inserting data into it, it's helpful to learn how to perform basic operations about collections. These basic operations are checking whether collection exists, creating and deleting collections. For all vector db opeations TROIA has VECTORDBACTION command.

VECTORDBACTION command gets action name, connection name and collection name for all operations. Other parameters depend on the action to be performed. 

{action} is the operation code that to be performed such as querying and listing, deleting, creating collections, querying etc. This action names are predefined.

{CONNECTIONNAME} is the connection that operation performed on. This parameters allows TROIA programmer working with multiple vector database connections simultaneously.

{COLLECTIONNAME} is the collection name that the operation performed on.




Checking Collection Existence
=============================

To check collection existence the action name must be COLLECTIONEXISTS. Here is the sample code:


::

	OBJECT:
		STRING CONNAME,
		STRING EID,
		STRING COLNAME,
		STRING MYERROR;

	CONNAME = 'MyConnection';
	EID = 'DEVQDRANT';             //this id is defined on SYST51
	COLNAME = 'testcollection';

	MAKEENDPOINTCONNECTION CONNAME ENDPOINTID EID;

	IF SYS_STATUS == 0 THEN
		VECTORDBACTION COLLECTIONEXISTS CONNECTIONNAME CONNAME COLLECTIONNAME COLNAME;

		IF SYS_STATUS == 1 THEN
			MYERROR = SYS_STATUS + ' ' + SYS_STATUSERROR;
		ENDIF;

		CLOSEENDPOINTCONNECTION CONNAME;
	ENDIF;
	
	
Listing Collections
===================

To get collection list, the action name is COLLECTIONLIST. Action returns a table that contains the list. Here is the sample code:


::

	OBJECT:
		STRING CONNAME,
		STRING EID,
		STRING MYERROR;

	CONNAME = 'MyConnection';
	EID = 'DEVQDRANT';           //this id is defined on SYST51
	
	OBJECT:
		TABLE TARGETTABLE;

	MAKEENDPOINTCONNECTION CONNAME ENDPOINTID EID;

	IF SYS_STATUS == 0 THEN
		VECTORDBACTION COLLECTIONLIST CONNECTIONNAME CONNAME INTO TARGETTABLE;

		IF SYS_STATUS == 1 THEN
			MYERROR = SYS_STATUS + ' ' + SYS_STATUSERROR;
		ENDIF;

		CLOSEENDPOINTCONNECTION CONNAME;
	ENDIF;



Deleting Collections
====================

To delete an existing collection, the action name must be DELETECOLLECTION. Here is the sample command.
Of course you must have a db connection before running VECTORDBACTION command like the examples above.

::

	VECTORDBACTION DELETECOLLECTION CONNECTIONNAME CONNAME COLLECTIONNAME COLNAME;


Creating Collections
====================

To create a new collection, the action name must be CREATECOLLECTION.
While creating a collection some extra parameters are needed these paremeters are read from a json formatted parameter.
These parameters may vary due to database product type. 
Here is the sample code for creating a collection on a Qdrant DB. For different product names and actual sample codes please review TROIA Help.


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
		VECTORDBACTION CREATECOLLECTION CONNECTIONNAME CONNAME COLLECTIONNAME COLNAME COLLECTIONPARAMS COLPARAMS;

		IF SYS_STATUS == 1 THEN
			MYERROR = SYS_STATUS + ' ' + SYS_STATUSERROR;
		ENDIF;

		CLOSEENDPOINTCONNECTION CONNAME;
	ENDIF;

For more and all supported opeations about collections please see TROIA help.


Inserting Data to a Collection
-------------------------------

If you have a large and long text, writing it directly to a vector database may not produce very efficient results. Therefore, **it is a more accurate method to divide the large data into smaller, meaningful pieces and add them to the database by calculating the "embedding" values ​​for each piece. These chunks are called "chunks". The performance of vector databases or AI models is directly proportional to how accurately you can transform data into chunks.**


Data Structure
==============

TROIA has a predefined data structure in table form for these "chunks" that you will import into the database. The essential columns in this data structure are as follows:

::

	ID		: chunk id, to distinguish each record
	TEXT		: text data
	EMBEDDINGS	: calculated embedding values
	
EMBEDDINGS column is a text column. It contains a comma separated decimal value list. Before inserting data to a vector database, this embedding values must be calcaulated with an "Embedding Model". Filling Embeddings will be discussed in next titles.

**You have to provide at least these tree columns to insert to a vector database.**


The secondary columns listed below are attached as payloads to the main vector database records. Each of these data points is an additional parameter to be passed to the vector database.

::

	BOOKID		: to store source book id
	SOURCE		: title/document or chapter under source the book
	LANGUAGE	: language, it is troia language code
	CHUNKINDEX	: number of the chunk that comes from same source
	QUESTION	: if it is possible, contains a question that is answered by "TEXT" column
	
You can also add other columns. All extra columns added to the chunk table are associated with the record as a payload and are added to the result after the lookup operation.


You can build this table structure using TROIA "APPEND COLUMN" command. Another useful alternative is using GETHELPS command. GETHELPS command exports all internal documents of TROIA Platform as chunks.
And it also have a variation that just creates empty table with this data structure. Here is the syntax:


::
	
	//EMPTY variation creates only data structure
	GETHELPS INTO {targettable} EMPTY; 
	
	//this variation creates structure and exports internal help data as chunks
	GETHELPS INTO {targettable};	

	
Here is a pseudocode that shows how you can build your chunk table easily
	
::

	OBJECT:
		TABLE MYCHUNKS;

	GETHELPS INTO MYCHUNKS EMPTY;
	
	//fill your rows to MYCHUNKS table
	
	//for each row
		//fill embeddings
		//insert to vector db.
		
		


Filling Embeddings
==================

To calculate the embedding values, firstly we need an "Embedding AI Model". 

To access the "Embedding AI Model" through the TROIA programming language, you need an LLM Gateway (which is a TROIA Platform Component) and some AI Model definitions on the LLM Gateway.

Since the LLM Gateway and how to define models on it are not directly related to the content of this chapter, you can read the relevant section of the book to understand these steps.In this section of the book, we will proceed assuming we have a properly configured and functioning LLM Gateway with an AI Model and an "Embedding AI Model" accessible through this configuration.


To connect a LLM Gateway and performn an operation on it, MAKEENDPOINTCONNECTION command is used very similar to connecting a Vector database. So you have to define an LLM Gateway on "SYST51 - Integration Endpoints Configuration" transaction. You can review the "Integration Endpoints" section to understand how this process works.

::

	OBJECT:
		STRING CONNAME;

	CONNAME = 'myLLMConnection';

	MAKEENDPOINTCONNECTION CONNAME ENDPOINTID 'DEVLLMGATEWAY';

	IF SYS_STATUS == 0 THEN
		
		//now llm gateway connection is ready to calculate embeddings.
		
		CLOSEENDPOINTCONNECTION CONNAME;
	ELSE
		LLMRESPONSE = 'LLM-Gateway Connection error. ' + SYS_STATUSERROR;
	ENDIF;


After connecting a LLM Gateway you can just use GETVECTOREMBEDDINGS() system function to calculate embedding vector your text data. Here is the syntax of GETVECTOREMBEDDINGS() command.


::
	
	GETVECTOREMBEDDINGS({endpointName}, {modelId}, {text}, {embeddingModel});
	
	{endpointName}	: endpoint connection name.
	{modelId} 		: ModelId defined on LLM-Gateway.
	{text} 			: The input text for which vector embeddings will be generated.
	{embeddingModel} : Embedding model name on LLM-Gateway.
	
	
{modelId} and {embeddingModel} parameters are dependent to the LLM Gateway configuration, so these paremeters must be ready before writing the code.
	
For more detail, please see TROIA Help for the function.

::

	OBJECT:
		STRING CONNAME,
		STRING MYMODELID,
		STRING MYTEXT,
		STRING MYEMBEDDINGMODEL,
		STRING LLMRESPONSE;

	CONNAME = 'myLLMConnection';
	MYMODELID = 'ias-ai-model';
	MYTEXT = 'This is a sample text.';
	MYEMBEDDINGMODEL = 'qwen3-embedding:4b';

	MAKEENDPOINTCONNECTION CONNAME ENDPOINTID 'DEVLLMGATEWAY';

	IF SYS_STATUS == 0 THEN
		LLMRESPONSE = GETVECTOREMBEDDINGS(CONNAME, MYMODELID, MYTEXT, MYEMBEDDINGMODEL);
		CLOSEENDPOINTCONNECTION CONNAME;
	ELSE
		LLMRESPONSE = 'LLM-Gateway Connection error. ' + SYS_STATUSERROR;
	ENDIF;


Inserting to Collection
=======================



Searching Data
--------------


