=============================
Working With Vector Databases
=============================

*A vector database is a type of database that stores data as mathematical “vectors” rather than using the traditional row/column structure, and is designed to perform similarity searches. It is especially used in the field of artificial intelligence. This section aims to explain how vector database operations can be performed with TROIA.*


Vector Similarity and Similarity Search
---------------------------------------

"Vector similarity" is the process of mathematically measuring how similar two vectors are to each other. The most common methods for calculating vector similarity are Euclidean Distance, Cosine Similarity, Dot Product etc. While cosine distance is commonly used in RAG and LLM applications, the calculation methods used may vary depending on the requirements. However, to understand the approach, you can think of it using the Euclidean distance method, which is used to calculate the distance between two simple points.

After understanding the concept of "Vector Similarity," let's imagine we convert any text into a sequence of numbers and compare different sequences to calculate their proximity. In this case, we can assume that the texts represented by two vectors that are close to each other are also close to each other.

By setting a threshold value for this proximity comparison and only displaying results that exceed that threshold, we are essentially performing a "similarity search".

It is clear that we should use the same approach when converting the texts we are working with for similarity searching into a new number vector.


What is a "Vector Database"?
----------------------------

...

What is "Embedding"?
====================

...

What is Semantic Search?
===========================

...

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


