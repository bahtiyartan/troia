

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
-----------------


License Server
--------------


How to Check JMX MBeans
=======================

...
