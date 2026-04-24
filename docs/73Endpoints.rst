=============================
Integration Endpoints
=============================

*TROIA platform is a flexible business application development platform with its integration facilities. Endpoint configurations is one of these integration facilities. This section aims to introduce integration endpoints infrastructure and related commands.*


What is an "Integration Endpoint"?
-----------------------------------

Endpoint is, in the simplest terms, an access point through which a system is exposed to the outside world. When you want to interact with an application (API, service, system), the address + target function you send a request to is called an endpoint.

From the perspective of the TROIA Platform, an endpoint is a definition you need to create when you want to access something outside of the TROIA Server.

In TROIA Platform builds 26.05.XX-XX and earlier, accessing external systems was done using specific TROIA commands without the concept and definition of endpoints. However, after this build, many of these accesses are done through endpoint definitions.

There are several advantages to accessing external systems through the concept and definitions of endpoints. The first is the ability to manage very similar configuration structures with a single application. Another advantage of endpoint definitions is the ability to define access restrictions on a profile and user basis by defining user permissions for endpoints.Finally, the ability to use the same commands and functions when connecting to different endpoint types makes the learning process easier.

How to Configure Endpoints?
===========================

Before establishing connections to an endpoint, you must create a definition for the endpoint. This definition requires its address, ports, protocols etc.

To make an endpoint configuration, you can use **"SYST51 - Integration Endpoints Configurations"** system transaction. On this transaction you can create a definition from scratch or clone en existing definition.

Where configurations stored?
============================

Endpoint definitions are stored on SYSENDPOINTS table. Some main data columns of SYSENDPOINTS table are listed below.

+-------------------+-----------------------------------------------------------------------------------+
| ENDPOINTID        | Id of an endpoint defintion. It is uniqe and used while establishing connections. | 
+-------------------+-----------------------------------------------------------------------------------+
| ENDPOINTTYPE      | Type of the endpoint.                                                             |
+-------------------+-----------------------------------------------------------------------------------+
| PROTOCOL          | Protocol which will be used while connecting.                                     | 
+-------------------+-----------------------------------------------------------------------------------+
| HOSTNAME          | Host for the connection. IP or DNS name supported.                                | 
+-------------------+-----------------------------------------------------------------------------------+
| PORT              | Port Number                                                                       | 
+-------------------+-----------------------------------------------------------------------------------+
| ENDPOINTUSER      | User or api id for authentication or other connection security processes.         | 
+-------------------+-----------------------------------------------------------------------------------+
| ENDPOINTPASS      | Pass or api key for authentication or other connection security processes.        | 
+-------------------+-----------------------------------------------------------------------------------+
| ENDPOINTPARAMS    | Other params to make endpoint connection. Format depends on the endpoint type     | 
+-------------------+-----------------------------------------------------------------------------------+
| STATUS            | Is it passive or active. Active is 1, passive is 0                                | 
+-------------------+-----------------------------------------------------------------------------------+

For more detailed and up to date list of SYSENDPOINTS table columns please browse it from DEVT01 - Table Management transaction.

Which Endpoint Types are Supported?
-----------------------------------

Endpoint types which are supported by the TROIA Platform are listed on *"Endpoint Type"* combobox while making a definition on SYST51. 

If you need supported endpoint types programmatically, GETENDPOINT TYPES() function returns whole list. The list that shown on "Enpoint Type" combobox on SYST51 is provided by this function already.

::

	OBJECT:
		TABLE TMPTABLE;
		
	TMPTABLE = GETENDPOINTTYPES();
	SET TMPTABLE TO TABLE TMPTABLE;
	RETURN;

Qdrant, RabbitMQ, Kafka, RedPanda, Mongo are main enpoint types that are supported. This list is updated with each new TROIA Platform build, for an up to date list of endpoint types that your TROIA Platform build supports, please see SYST51 transaction.

**The TROIA Platform offers many integration options beyond the endpoint types listed in SYST51. The options listed here are only integration options created using the "Endpoint" infrastructure.**


Authorization for Endpoint Configurations
=========================================

Establishing connections to endpoints and transferring data requires endpoint access permission for the user performing the operation.

To grant access permissions to endpoints, the **"SYST51 - Integration Endpoints Configurations"** application is used again.



Managing Endpoint Connections
-----------------------------


Creating New Connections
===========================


Closing Connections
===========================


Performing Operations on an Endpoint Connnection
================================================





