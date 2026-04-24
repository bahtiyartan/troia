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

**Qdrant, RabbitMQ, Kafka, RedPanda, Mongo** are main enpoint types that are supported. This list is updated with each new TROIA Platform build, for an up to date list of endpoint types that your TROIA Platform build supports, please see SYST51 transaction. Some future components of the TROIA Platform will also be integrated into the system via endpoint definitions.

**The TROIA Platform offers many integration options beyond the endpoint types listed in SYST51. The options listed here are only integration options created using the "Endpoint" infrastructure.**


Authorization for Endpoint Configurations
=========================================

Establishing connections to endpoints and transferring data requires endpoint access permission for the user performing the operation.

To grant access permissions for the endpoints, the **"SYST51 - Integration Endpoints Configurations"** application is used again. Permissions can be defined for users or profiles within this application. 



Managing Endpoint Connections
-----------------------------

A connection is required for data exchange or, depending on its purpose, to perform an operation at an external endpoint. Depending on the endpoint type, automatic connections can be established by the TROIA infrastructure to certain endpoint definitions. Some endpoints require connection to be established at the development level within the TROIA code flow. In this case, the process of closing these connections must also be managed by the application level. In both cases, the TROIA Platform attempts to close open endpoint connections before logging out.

It is possible to connect to different endpoints simultaneously. Furthermore, multiple connections can be established to the same endpoint at the same time. To enable this flexibility, a virtual name must be assigned to a connection when establishing one. This connection name must be used in every operation performed through that endpoint connection.


Creating New Connections
===========================

To create a new connection, you must use MAKEENDPOINTCONNECTION. This command creates a connection to given endpoint. This endpoint id is the same value that you defined in SYST51 and stored in SYSENDPOINTS.ENDPOINTID column. System gets all required data from endpoint definition.

::

	MAKEENDPOINTCONNECTION {connectionname} ENDPOINTID {endpointid};
	


Closing Connections
===========================

When you establish a new connection, you should close it as soon as possible. In other words, the lifespan of a connection should be as short as the time interval it is needed.

For connections established using the TROIA command, the longest lifetime is the time between the opening and closing of the transaction. When the transaction is terminated, the TROIA Platform automatically closes the connection even if it remains open until transaction close.

::
	
	CLOSENDPOINTCONNECTION {connectionname};


Connecting && Disconnecting Sample
==================================

Here is a connection example:

::

		OBJECT:
			STRING MYCONNECTIONNAME,
			STRING MYENDPOINTID;
		
		MYCONNECTIONNAME = 'Connection1';
		MYENDPOINTID = 'DEVQDRANT';
		

		MAKEENDPOINTCONNECTION MYCONNECTIONNAME ENDPOINTID MYENDPOINTID;

		IF SYS_STATUS == 0 THEN

			//do your endpoint actions here

			CLOSEENDPOINTCONNECTION MYCONNECTIONNAME;

			IF SYS_STATUS == 1 THEN
				STRINGVAR3 = SYS_STATUS + ' ' + SYS_STATUSERROR;
			ENDIF;
		ELSE
			STRINGVAR3 = SYS_STATUS + ' ' + SYS_STATUSERROR;
		ENDIF;




Performing Operations on an Endpoint Connnection
================================================

Communication with an endpoint can only be performed after establishing a connection and before closing it.

This process varies depending on the type of endpoint being connected. You must use the correct command in the TROIA programming language according to the type of connection. For example, while VECTORDBACTION is used for the Qdrant Vector database, BUSACTION can be used for Kafka or RabbitMQ. To determine the correct command, you can refer to the current TROIA help files or the appropriate sections in this book.


Common Problems about Endpoint Connections
------------------------------------------

