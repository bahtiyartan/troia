

=========================================
Monitoring with JMX
=========================================

*JMX is a Java technology used to monitor and manage Java applications while they’re running. This section aims to explain how to read monitoring data using JMX from TROIA Platform Components.*


What is JMX?
============

JMX (Java Management Extensions) is a Java technology used to monitor and manage running Java applications. It is a standard mechanism that allows us to observe the internal state of a running Java application from the outside and manage it when necessary.

It allows monitoring JVM information such as RAM/heap usage, number of threads, garbage collection, and CPU. It can also export application-specific metrics via a structures called MBeans.

JConsole, VisualVM, and Prometheus JMX Exporter are the widely used tools to monitor Java Applications that supports JMX.

TROIA Platform is also a java based system so it is possible to observer TROIA Platform components using JMX as any java application. Some components such as License Server and Application Server also provides some special MBean data in addition to attributes that a standart Java appliation provides.

How to Enable JMX?
==================

To enable JMX for a JVM you must provide required JMX parameters, you must at least provide jvm parameters below:

::

    -Dcom.sun.management.jmxremote
    -Dcom.sun.management.jmxremote.port=YourJMXPortNumber
    -Dcom.sun.management.jmxremote.authenticate=false
    -Dcom.sun.management.jmxremote.ssl=false


In production environment you must make more secure configuration, with ssl and authentication options, or port restriction methods. But for local envirionments this minimal configuration is enough for testing. **For production, please review all security options and port restriction methods about JMX.**

TROIA Platform components checks **com.sun.management.jmxremote** and **com.sun.management.jmxremote.port** system properties to decide whether jmx is enabled or not. If these two parameters are provided for the vm, system enables MBeans which are related with TROIA PLatform.


TROIA Specific MBeans
=============================

All TROIA Platform components supports JMX, but Application Server and License Server also provides custom JMX MBean attributes about their internal operations.

Application Server
------------------

Application Server provides a JMX MBean named **ApplicationServer** under **com.ias.server.appserver.monitoring.jmx** package and this bean contains some paraters about application servers state. Here is the list of the attributes:

+-----------------------------+-----------------------------------------------------------------------+
| ServerSessionCount          | Session Count on Application Server                                   |
+-----------------------------+-----------------------------------------------------------------------+
| LoginDuration               | Last Login Operation Duration (ms)                                    |
+-----------------------------+-----------------------------------------------------------------------+
| TransactionOpeningDuration  | Last Transaction Opening Duration (ms)                                |
+-----------------------------+-----------------------------------------------------------------------+
| DialogOpeningDuration       | Last Opening Duration (ms)                                            |
+-----------------------------+-----------------------------------------------------------------------+
| ClassCacheSize              | Current Size of Application Server Class Cache                        |
+-----------------------------+-----------------------------------------------------------------------+
| DialogCacheSize             | Current Size of Application Server Dialog                             |
+-----------------------------+-----------------------------------------------------------------------+

These attribute list may vary, due to your TROIA Platform build number. For more and up to date list please check actual JMX Bean for your build.

License Server
---------------

Another TROIA Platform Component that provides some special data for JMX is LicenseServer. JMX Bean Name is **LicenseServer** and it is under the "com.ias.server.controller.monitoring.jmx". The attributes provided by the License Server are slightly different from those provided by the Application Server, because License Server has dynamic attribute names due to your license definitons. But prefixes are always same here is th list:

+-----------------------------+-----------------------------------------------------------------------+
| MaxRegularUserCount         | Maximum Number of Regular Users                                       |
+-----------------------------+-----------------------------------------------------------------------+
| CurrentRegularUserCount     | Current Regular User Count (currently consumed regular user licenses) |
+-----------------------------+-----------------------------------------------------------------------+
| AvailableRegularUserCount   | Available Regular User Count                                          |
+-----------------------------+-----------------------------------------------------------------------+
| MaxPortalUserCount          | Maximum Number of Portal Users                                        |
+-----------------------------+-----------------------------------------------------------------------+
| CurrentPortalUserCount      | Current Portal User Count (currently consumed portal user licenses)   |
+-----------------------------+-----------------------------------------------------------------------+
| AvailablePortalUserCount    | Session Count on Application Server                                   |
+-----------------------------+-----------------------------------------------------------------------+

If you have multiple licenses defined, system creates multiple items with same prefix. For example if you have two licenses with id **IAS4-26-0202114159-10** and  **IAS1-26-0202114159-20** you will have MaxRegularUserCount:IAS4-26-0202114159-10 and MaxRegularUserCount:IAS1-26-0202114159-20. So all these attributes will be repeated for each of your licenses.

Number of these attributes may vary due to your TROIA Platform build number and your license type etc. For the exact list please License Server JMX on your own environment.


How to Check JMX MBeans
=======================

JMX is supported for too many monitoring tools to monitor java applications. 
