

=========================================
Basic Java Concepts For TROIA Developers
=========================================

*TROIA Platform works on Java Virtual Machine, so to know about Java saves time in some special cases such as performance optimization, debugging and deployment. This section aims to introduce basic and useful java concepts for troia developers or system administrators.*


What is Java?
-------------

Java is a simple, high-level, object-oriented, general-purpose programming  language. It works on a virtual machine which is called JVM. So java applications work on anywhere which JVM works on, so it is a "write once, run anywhere" programming language.

Java offers some economic and technical advantages and all this advantages become a part of the solutions that are devloped with Java. Here are some of the pros of Java programming language.


**Object oriented :** enhances the flexibility and reusability of the code.

**Readible :** it is a high-level programming language, so it is human readible and reduces maintenance costs.

**Platform-independent :** java applications work on anywhere which JVM works on, so they are portable.

**Supports multithreading :** multithreading helps us to gain the maximum utilization of CPU and allows handling larger data and more complex problems in relatively short running time.

**Simple and secure :** Memory management is handled by garbage collector, there is no complex concepts like pointers, it is easy to learn.


Where java codes stored?
========================

Java codes are stored in files which has **.java** extension. These files can be edited via complex IDEs (eclipse,netbeans etc) or simple editors (notepad, vi). In a large application, there can be numerous .java files. 


How java programs run?
======================

Java is combination of compiled and interpreted languages. .java files are compiled by javac which is a component of JDK and .class files are generated. These .class files contains bytecodes which can be interpreted for the underlying platform that the application runs on. 

This interetation process is handled by JVM (Java Virtual Machine). This interpretation process isolates, .class file which is the result of compilation proces and the host machine. .class files are loaded by class loader and executed by Execution Engine. JVM has complex subcomponents such as class loader, execution engine, garbage collector, runtime memory etc., but these components and their running metodology is out of our scope.


JVM is a subcomponent of JRE.


Java SE/EE/ME
==============

Java platform has multiple packages for different devices and operating systems. Java SE provides the core functionality of the java programming language such as basic types and objects of networking, security, database access, graphical user interface etc. 

Java EE platform and provides an API and runtime environment for developing and running large-scale, multi-tiered, scalable, reliable, and secure network applications. Applications developed using Java EE can run on servers such as Tomcat, GlassFish, JBoss.

Troia platform has different components which aims to run on Java SE and Java EE. Java ME allows running java applications on small devices and it is out of our scope.


JRE / JDK
----------------

JRE (Java Runtime Environment)
==============================

JRE is a software layer that runs on operating system and provides class libraries, virtual machines, and other tools that a particular java application needs to execute such as deployment solutions (Java Web Start), development tookit (java 2D, awt, swing),	integration libraries (JDBC,JNDI), 	language and utility libraries (logging, concurrency utils etc). JRE becomes operational at the moment when the application program is executed.

In windows operating system, installed JRE versions are listed Control Panel-> Java -> Java Tab (Viev Button)

.. figure:: images/java/installed-jre.png
   :width: 650 px
   :target: images/java/installed-jre.png
   :align: center
   


JDK (Java Development Kit)
===============================

JDK is the key platform component for **building** Java applications, so it contains java compiler, java doc and some optimization tools. JDK contains JRE, so if you install JDK you can build and run java applications. 


Here is a simplified graph that shows the content of JDK and JJRE and their main subcomponents:

.. figure:: images/java/java-jdk.png
   :width: 385 px
   :target: images/java/java-jdk.png
   :align: center




Java Releases
-------------


Open JDK / Oracle JDK
====================

OpenJDK is a free and open-source implementation of the Java SE Platform Edition. It is developed by Oracle and the Java Community. The other and official alternative is Oracle JDK. Oracle JDK is developed based on OpenJDK, so they are technically very similar, even though some minor differences. Oracle claims that Oracle JDK has better performance and stability than Open JDK. 

Although one of the main differences between these two releases, Oracle JDK seems more stable hand responsive especially for enterprise usage.

Both Open JDK and Oracle JDK has different releases and versions, for now Oracle JDK 1.8 is the most used jdk release which is the last release published before Oracle's license strategy change. TROIA Platform works properly with Open JDK (after Open JDK 11) and Oracle JDK releses (oafter Oracle JDK 1.8.x)



What is Your Java Release?
==========================

.. figure:: images/java/java-version.png
   :width: 650 px
   :target: images/java/java-version.png
   :align: center
   

.. figure:: images/java/java-version-openjdk.png
   :width: 650 px
   :target: images/java/java-version-openjdk.png
   :align: center


Java Memory Management
-----------------------


Garbage Collector
===================

Memory management in Java is handled in the background by the Garbage Collector which is a sub component of JVM. This is one of the most important features that makes java more writable and stable and of course more pupular.

Because developers can create new objects without worrying memory management, the garbage collector does the memory allocation and deallocation. In languages like C/C++, developers have to control all these processes and these processes consists too much risk for the application stability. From this aspect java offers great value.


Managing Memory In TROIA
========================


JVM Arguments
-------------







Some Basic Java Terms
---------------------

ClassPath
=========

.java Extension 
===============

ByteCode and .class File Extensions
===================================

.jar and .war Extensions
=============================

JNLP / codebase
================

Java Console
============


Fonts in Java
==============



Environment Variables and JAVA_HOME
-----------------------------------


Understanding Java Exceptions
-----------------------------

Java Exception Types, Compile/RunTime/Errors

Exceptions/Error

NullPointerException

ArrayIndexBound

ParseException

NoSuchElement

ClassCastExcept

ClassNotFoundError



Logging
-----------------------------


Optimization Tools
------------------



Monitoring Tools
-----------------



#. . . 
#Server JRE

References

*https://docs.oracle.com/javaee/6/firstcup/doc/gkhoy.html*
https://www.theserverside.com/definition/JAVA_HOME#:~:text=JAVA_HOME%20is%20an%20operating%20system,JDK%20or%20JRE%20was%20installed.
https://www.ibm.com/cloud/learn/jre
https://www.freecodecamp.org/news/jvm-tutorial-java-virtual-machine-architecture-explained-for-beginners/#:~:text=The%20JVM%20consists%20of%20three,Execution%20Engine













	