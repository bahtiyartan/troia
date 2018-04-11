

=======================
Platform Basics
=======================

*TROIA Platform is the software framework which TROIA Programming Language works on. This section introduces main components and architecture of TROIA Platform for better understanding of next sections.*

Components and Network Architecture Overview
============================================

Application Server
------------------

Application server is the most important component of TROIA platform. As it is obvious, it is a server side service that serves for all kind of TROIA clients. Basically application server is responsible for user session management, handling troia application lifecycle, database transaction management etc. 

The most important responsibility of application server is running TROIA programming language and handling troia application lifecycle. **Application server is the only platform component that is able to run TROIA codes.** Therefore it is not possible to run a troia code without an application server.

A single application server is able to serve multiple clients. User count that an application server is able to serve properly depends on the workload of application server. It is possible to run multiple servers simultaneously.

Client(s)
---------

It is not possible to log in an application server and run a troia application directly on application server. To do this operations you need a TROIA client. Most used TROIA client is desktop client (also called java client or swing client) that is able to connect application server and draw troia applications on user interface. The main functionality of this client is getting user actions passing them to application server and drawing resulting screen to user interface. Additonally, development environment of TROIA Language is a part of java client, in other words to develop a TROIA Application you have to connect application server via a desktop client.

There are different kind of clients such as android client, web client and web services. Clients are not able to run TROIA codes, their basic responsibility is transferring user interactions to server and handling application server response.

License Server
--------------

License Server, a server side service that handles licencing issues considering user count and modules (TROIA Application groups). In general, License Server serves for application servers, although some other server side components needs license server.

After its launch, an application server tries to access License Server at first login attempt. If your License Server is down or not accessible, application servers do not allow users to log in. If license server gets down while application servers have already logged users, this users can work properly. But it is not possible to log in new users until your application server access license server.

To serve properly, license server and the application servers that it serves for must have same version.

RMI Registry
------------

RMI Registry is a server side service that provides a communication infrastructure between all components of TROIA Platform. RMI Registry uses RMI Infrastructure of java which provides cummunicatation method for different java applications. Any two components of the platform uses RMI Registry (server to server, client to server, server to license server etc), so running a single RMI registry is a must to run other platform components properly.

Other Components
----------------

Although most important tools and components are listed above, TROIA Platform has other components for load balancing, administration, single sign on, monitoring, SMS handling etc. Some of this components are listed below:

**Load Balancer** is a server side service that redirects clients to best available server considering server resources and the rules that are defined its configuration.

**SSO Gateway** is a server side service that provides single sign on options for TROIA Platform users.

**SMS Gateway** is another server side service for handling SMS handling.

**Workbench** is an administration tool(a kind of client) to manage and monitor server side components such as application server,license server,load balancer. Using this tool system administrators can view and manage application server cache, users sessions and their running applications etc.

**System Reporter** is a tool that reports status of your server side components in a configurable period.

Network Architecture Overview
-----------------------------

.. figure:: images/platformbasics/troia-platform-network-architecture.png
   :width: 600 px
   :target: images/platformbasics/troia-platform-network-architecture.png
   :align: center


Software Architecture Overview
==============================

Basically, TROIA Platform allows programmer to develop applications that access data, manipulate it and save. To understand platform architecture its very important to understand the roles of application server and client(s). The graphic below, shows the main structures and layers of TROIA Platform from user interface to data. Also, it's possible to understand responsibility/functionality share of application server and client. 

.. figure:: images/platformbasics/software-architecture.png
   :width: 600 px
   :target: images/platformbasics/software-architecture.png
   :align: center

   
Understand the roles of each component and their subsystems using this graphic and try to create some statements and discuss them for better understanding. Here are some sample statements to discuss:

::

	- TROIA Codes are executed only in application server.
	- IDE is a part of desktop client, 
		so TROIA applications are only developed desktop devices.
	- Desktop, web and android client shares same troia application model layer.

About Deployment
================

How to Read Version Number?
---------------------------

As an TROIA application developer or system administrator, it is important to know your version number. Because some new features revealed or bugs are fixed with new releases. And you must know whether your version supports the features that you need or all your components have same version to be sure that your installation is valid or not.

You can read your version number from the about dialog (Menu->About). Sample version numbers are listed below:

::

	3.08.05 021101 
	5.01.02 012102 
	5.02.04 041201 
	
TROIA Platform version numbers are consisted from two main parts "major version number" and "build number". As usual, version numbers are ordered, for example 3.08.05 0121101 is an older version than 5.02.04 012102, 5.02.04 101001 is younger than 5.02.04 090901.

602, 603 and 604 are names **CANIAS ERP** versions and all are designed to run on a major TROIA Platform version. (602 works on 3.08.xx xxxxxx, 603 works on 5.01.xx xxxxxx and 604 works on 5.02.xx xxxxxx). In other words; 602,603,604 are not valid version names for TROIA Platform.


How to Follow Changes & Improvements?
-------------------------------------

Each version of TROIA Platform fixes some bugs or reveals some new features in different layers. In some cases, version upgrade requires some manual operations by administrators or developers. So you need to follow changes between version upgrades. All changes are listed in ReleaseNotes.txt document which is supplied/distributed with each version. Also it is possible to read release notes document from "SYST17 - Release Notes" TROIA application and "Relese Notes Analyser" tool on Workbench.






