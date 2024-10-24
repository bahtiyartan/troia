

=========================================
VisualVM for Optimization and Monitoring
=========================================



Profiling Tools
-------------

A profiling tool is an optimization tool which is used to determine resource problems about components of the sofware such as which components of software consume the most resources, which functions run the longest, and which processes cause bottlenecks. 

Profiling tools focus primarily on performance analysis and memory management, because the primary bottleneck resources in a software systems are the CPU and memory. So in generalprofiling tools generally do not focus on resources such as disk or network.

Performance and memory analysis are performed to detect and resolve various errors or to optimize problematic parts of the code in terms of processor and memory usage. All profiling tools work for these purposes, however, they vary according to the characteristics and needs of the programming language or platform.

Performance (CPU) Analysis
==========================

Performance analysis is the whole analysis process based on the running time, such as the triggering order of functions within the application, the number of triggers, and the time spent in these functions. It measures the speed of the application and determine its efficiency in terms of processing load.



Memory Analaysis
================

Memory Analysis is the analysis process carried out to measure the memory usage of the application, examine the data held in memory, and detect unnecessary memory usage or memory leaks.


What is VisualVM?
-----------------

Java applications run on a virtual machine called the "Java Virtual Machine" (JVM), which provides a platform-independent runtime environment for Java programs. It provides many tools for analyzing and using applications running on it.

VisualVM is a profiling tool used to monitor and analyze applications running on the JVM. Structurally, it offers a user-friendly interface with all the profiling tools offered by the JVM. Since it offers some limited features in terms of monitoring, it can also be used as a monitoring tool when needed.


Main Functionalities of VisualVM
================================

It is possible to perform the following operations on VisualVM:

- Monitoring application's CPU and memory usage data in real time
- Monitoring threads on the application and their status
- Analysing which class functions consume more CPU
- Take a dump of the class instances in memory
- Analyzing memory/heap dumps
- Trigger the Garbage Collector operation or watch the moments when it is automatically triggered


Alternative Tools
=================

As an alternative to VisualVM, YourKit is one of the most common and professional options. Other solutions such as JProfiler or Java Mission Control can also be considered as alternatives.


How to run VisualVM?
-----------------

First, you should download VisualVM from https://visualvm.github.io/. After opening the compressed folder you downloaded, you can run the \bin\visalvm.exe file in this folder. For other operating systems, you need to follow the necessary instructions according to the structure of your system.

Yapılacak incelemeye niteliğine göre VisualVM başlatılırken, bellek parametrelerini ve VisualVM’in çalışacağı JDK path’ini de konfigüre etmek gerekebilir. Bunun için \etc\visualvm.conf dosyası içindeki ilgili parametreleri ihtiyaçlarınıza göre düzenlemelisiniz. Bu configrürasyon dosyası içindeki önemli parametreler, visualvm_default_options ve visualvm_jdkhome parametreleridir. Diğer parametreler için VisualVM dokümantasyonunu inceleyiniz.


.. figure:: images/visualvm/visualvm_conf.png
   :width: 650 px
   :target: images/visualvm/visualvm_conf.png
   :align: center
   
For the same purpose you can also enter -J<jvm_option> and --jdkhome parameters to the application while running VisualVM without changing the configuration file. Further, many operations can be performed via the interface with the VisualVM command line tools, and thus processes can be automated. You can examine the other VisualVM parameters by running the visualvm –help command in the folder where the VisualVM application is located. 

The screen that appears when the application is running has two basic components. The first of these is the left section, which shows the running JVMs, snapshots or heapdumps. The other is the second section, which shows the status of the connected JVM or the operations to be performed on this JVM. When the application is first opened, the splash screen or a blank screen will appear because there is no connected JVM yet.

.. figure:: images/visualvm/visualvmhome.png
   :width: 650 px
   :target: images/visualvm/visualvmhome.png
   :align: center

It is also possible to connect to a remote JVM via VisualVM. In this document, we will focus only on operating on a locally running JVM.
   


JDK (Java Development Kit)
===============================

..

.. figure:: images/java/java-jdk.png
   :width: 385 px
   :target: images/java/java-jdk.png
   :align: center













	