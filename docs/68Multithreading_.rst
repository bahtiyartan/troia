

========================
Multithreading
========================

*TROIA Platform supports parallel computing with it's 8.02 version. This section aims to introduce you parallel computing && related concepts in TROIA*

What is Parallel Computing?
--------------------------

...


Parallel Computing Concepts in TROIA
------------------------------------

..

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


Running A Transaction on Another Thread
=======================================

	
Waiting for the Thread Group to End
-----------------------------------

...


Defining Semaphores
-------------------


ACQUIRESEMAPHORE {semaphoreid} [SCOPE SERVER | SYSTEM]
RELEASESEMAPHORE {semaphoreid} [SCOPE SERVER | SYSTEM]



Useful Functions & Commands
---------------------------

ISTHREADGROUPALIVE()


Some Facts About MultiThreading on TROIA
----------------------------------------



