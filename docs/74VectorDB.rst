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

...

**Qdrant**


Managing Collections
--------------------

check collections

delete collections

create collections


Inserting Data to a Collection
-------------------------------


Data Structure
==============


Filling Embeddings
==================


Inserting to Collection
=======================



Searching Data
--------------


