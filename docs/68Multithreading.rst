

=============================
Concurrency (Multi-Threading)
=============================

*TROIA programming language supports concurrency (multi-threading) computing onwards 8.02 version of the platform. This section aims to introduce you concurrent programming and related concepts in TROIA*


What is Concurrency?
-----------------------------

Concurrency is, performing more than one operation at the same time. If a software is concurrent it means that multiple sequences of operations are run in overlapping periods of time. In concurrent programming, it is not guaranteed that multiple operations are performed simultaneously, so in a specific period, one of the threads can wait for others to perform their operations. 

One of the most important concepts of concurrent programming is "common resources" that multiple threads share like sensitive variables and functions etc. Multiple threads race to modify and read these shared resources. Therefore, programmers have to organize all threads to use this shared resources in a safe and secure way. For more information about concurrent programming (multi-threading) and related concepts, see related articles and books.


Concurrency Concepts in TROIA
-----------------------------

Like most programming languages TROIA, supports conccurrency thanks to TROIA threads. A TROIA thread is a kind of sequence of program instructions that can be managed independently by TROIA runtime. In conventional 
troia programming, there is only one thread that all instructions executed on. In concurrent programming approach, programmers can define  multiple threads that performs a specific task. In TROIA threads can be managed in thread groups that can contain one or more threads.

As mentioned before shared resources consist an important problem that must be managed by programmers, however in TROIA some variables like SYS_STATUS, SYS_STATUSERROR are global in transaction scope, by design. This is kind of obstacle or limitation for threads in transaction scope. Because of this limitations, **it is not possible to create multiple threads in a single transaction**, so each thread must have its own transaction. To start a new thread and perform some tasks in this thread CALL TRANSACTION command is used. This is also useful for separating other common resources like trace files etc.

Another shared resource is database connection, as mentioned in previous sessions database connection is shared between all transaction. This is another problem to write an concurrent TROIA application. To overcome this problem transactions that is executed by alternative threads must have their own database connection. To open a transaction with dedicated database connections, EXCLUSIVEDB keyword of CALL TRANSACTION command must be used while calling transactions as new threads.

Now, lets discuss all these concepts in detail with examples.


Thread Groups
=============

Thread group is a kind of label to categorize a couple of threads in a single group. It allows programmers to create a common behavior for all threads in a group. Assume that you have four threads that performs parts of task and you want to wait for all these four threads to finish to run another task. In this case, you must label all your threads with same label to wait all of them without checking state of each thread. 


Here is a more complex example of thread groups, please try to understand the behavior of pseudocode

::

    - start thread on group1
    - start thread on group1

    - start thread on group2
    - start thread on group2

    - wait until group1 ends
    - do something (1)
    - wait until group2 ends
    - do something (2)

In this case, system starts two threads on firts group and additional two threads in another group. Then, waits first group to end to do first do somehing block. When first two threads ends, program moves to fist do something block while second group is still running. After threads on second group ends it moves to second do somehing block.


Each troia thread must have a group name, but it is possible to define only one thread in a group. So, it is also possible to manage troia threads individually.


Creating TROIA Threads
----------------------


To start a new TROIA thread, firstly you need to call a new transaction in server with INSERVER variation of CALL TRANSACTION command. Calling a transaction in server, opens the transaction, starts first dialog it with BEFORE and AFTER events and then calls TRANSCALLED function if there is at least one parameter and calls TIMEPROCESS event due to given parameters on CALL TRANSACTION command. For more information about calling a transaction in server please review related section.

To call transaction with a new thread you must indicate and thread group for the called transaction with the ONTHREADGROUP keyword and EXCLUSIVEDB parameter to establish dedicated database connections for first and second database connections. For more information about these two database connections please see related section. Here is a simplified syntax for calling transaction in a thread.

::


	CALL TRANSACTION RDTT06 INSERVER [({inputparams})] ONTHREADGROUP {tgroupid} EXCLUSIVEDB;
	

If you don't define a thread group for transaction call, it is considered as an conventional single thread call transaction and does not performed by a  new thread. EXCLUSIVEDB parameter is not a must but if you need some database operations in your threads, it can cause some subtle problems, so it is recommended to use establish dedicated database connections for transaction called in a thread.



After calling a transaction with its own thread your code flows to the next line without waiting end of transaction call, if you need to wait all transactions in a thread group to end for performing another task you must use JOINTHREADGROUP command. Here is the syntax for it:


::

	JOINTHREADGROUP {tgroupid};
	
Waiting for all thread groups before returning to client is must, otherwise it is possible to encounter some subtle problems about threads lifecycle.


Here is a simple example below puts all of them together. In this example; THREADID is not a special variable or reserved word, it is just a function parameter to pass a distinctive information for each transaction.


::


	OBJECT:
	    STRING THREADID;

	TRACEON;
	
	THREADID = 'T1';
	CALL TRANSACTION RDTT06 INSERVER (THREADID) ONTHREADGROUP 'TG1' EXCLUSIVEDB;
	
	THREADID = 'T2';
	CALL TRANSACTION RDTT06 INSERVER (THREADID) ONTHREADGROUP 'TG1' EXCLUSIVEDB;
	
	THREADID = 'T3';
	CALL TRANSACTION RDTT06 INSERVER (THREADID) ONTHREADGROUP 'TG1' EXCLUSIVEDB;

	JOINTHREADGROUP 'TG1';

	TRACEOFF;


Assume that TRANSCALLED function of RDTT06 transaction's first dialog is just a "DELAY 10000;" (sleep for ten seconds). All this code takes about ten seconds. And TRACEOFF command runs when all threads end. But if you remove ONTHREADGROUP code takes at least thirty seconds, because all call transactions executed sequentially.
	


Defining Semaphores
-------------------

::


	ACQUIRESEMAPHORE {semaphoreid} [SCOPE SERVER | SYSTEM]
	RELEASESEMAPHORE {semaphoreid} [SCOPE SERVER | SYSTEM]



Useful Functions & Commands
---------------------------

::

	ISTHREADGROUPALIVE()


Some Facts About MultiThreading on TROIA
----------------------------------------



