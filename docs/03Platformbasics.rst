

=======================
Platform Basics
=======================

*TROIA Platform is the software framework which TROIA Programming Language works on. This section introduces main components and architecture of TROIA Platform for better understanding of next sections.*


#troia platform purpose

History
--------------------

#history of troia platform

Basic Components
----------------

Application Server
==================

#application server


Client(s)
=========

#client

Licence Server
==============

Licence Server, a server side service that handles licencing issues considering user count and modules (TROIA Application groups). In general, Licence Server serves application server, although some other server side components needs licence server.

If your Licence Server is down or not accessible, application servers do not allow users to log in. After its launch, an application server tries to access Licence Server at first login attempt. To serve properly, licence server and the application servers that it serves for must have same version.

Load Balancer
=============

#load balancer

RMI Registry
============

#rmi registry


Other Components
================

#other compoenents

Platform Overview
--------------------

.. figure:: images/platformbasics/platformoverview.png
   :width: 700 px
   :target: images/platformbasics/platformoverview.png
   :align: center


Platform & Programming Language
-------------------------------

#programming language







