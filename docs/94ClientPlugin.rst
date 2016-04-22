

=========================
Client Plugin Development
=========================

*TROIA Platform offers an api to integrating third party applications with TROIA client. This sections aims to introduce client plugin api to developers who wants to develop applications integrated with client application of TROIA Platform.*


Introduction
------------

Plugins are third party applications or libraries that are able to communicate with Canias Client over plugin service. It is possible to develop a wide range of plugins from simple logging applications to enterprise level business intelligence tools.


Enabling Plugin
===============

To enable plugin check Menu -> Settings -> Plugin -> Enable Plugins checkbox.

Installation Details
====================

If plugin service is enabled, client deploys and starts Plugin Service automatically. Plugin Service is installed to {userhomefolder}\RESOURCES\PluginService\ folder. PluginService is also updated by Canias Client. Plugin Infrastructure is supported on 5.02.01 100801 and following versions.


Components of Plugin Infrastructure
===================================

**Plugin Service:** A standalone service that creates a communication infrastructure between client and plugins. This service is installed and run automatically if there is an open Canias client that allows plugin service (Canias-> Settings -> Plugin->Enable Plugins) 

**caniasplugin.jar:** The library for developing all types of Canias Plugins. This jar is supplied by IAS.

Types of Plugins
----------------

There are two types of Canias Plugin. First one is executable plugin. Executable plugins are standalone and runnable applications that connect to Canias. This kind of applications must connect to Canias plugin service using caniasplugin api. For example; business intelligence tools, drawing or cad tools.

Second type of plugins is non-executable plugin. This plugins are not runnable applications. They are installed to plugin service as library file. Plugin service fires (calls) their methods when a related action occurs. In this type of plugins there is no need an extra running application. For example: logging applications, etc.


How to Develop a Plugin
-----------------------

To connect a standalone application to Canias or develop a Canias Plugin caniasplugin.jar must be used as library. This api contains all required infrastructure communicate with Canias Clients. Using this api it is possible to send actions to Canias clients or listen actions from Canias Client. Two sample plugins(executable, non-executable) are supplied with this document.
 
Key Classes
===========

To develop a Canias Plugin, programmers must only inherit iasAbstractPlugin class which is supplied in caniasplugin.jar.

iasAbstractPlugin has abstract methods that must be overridden by your plugin. When and PLUGINACTION command run on an TROIA application, doAction(iasPluginAction p) method is fired. To send events to Canias Client from a plugin use postEvent(iasPluginEvent e, String target) method.

iasPluginAction class contains all required information such as action class, action type, action value, session id, transaction id, username and transaction. Event source is also contained by iasPluginAction. Using event source value, it is possible to send events to Canias Client which sends previous action.
To fire an action on Canias Client, plugins must send an iasPluginEvent using postEvent() method of iasAbstractPlugin. Event type and event value must be supplied by plugin. 

Details about all required classes are in Javadoc files which are supplied with this document and caniaspluging.jar. Also, sample plugins include usage examples of this classes.

Deploying a Plugin
==================

To deploy second type of Canias Plugin (non-executable) plugin jar must be copied into …\RESOURCES\PluginService\plugins\ folder. For non-executable plugins jar file must have same name with the class that inherits iasAbstractPlugin. For example; if com.samplecompany.sammpleplugin.java  inherits iasAbstractPlugin jar’s name must be com.samplecompany.sammpleplugin.jar (See sample plugins Non-Executable)

Executable plugins are standalone applications, so there is no need to any special deployment action.


Sending Message to a Plugin
---------------------------

PLUGINACTION Command
====================

For sending an action to plugins from TROIA Layer, PLUGINACTION command is used, here is the syntax:

::

	PLUGINACTION ACTIONCLASS {actionclass} ACTIONTYPE {actiontype}
	                              [ACTIONVALUE {value}] [TARGET {pluginid}]

Action class shows plugin functionality. Plugin Service sends this action to related plugins using action class parameter.  In other words it is used for selecting which plugin must consume this action. Second parameter, action type is used to determine which action will be performed on selected plugin. Action value is optional and it's value is passed to plugin as string. (pure string, xml, json, etc.)

Its also possible to send an action to a target plugin directly without plugin selection process. Target parameter is used to indicate target plugin for action. To get pluginId of a executable/non-executable plugin, PLUGINVALIDATE command is used.

PLUGINVALIDATE Command
======================

::

	PLUGINVALIDATE [ACTIONCLASS {actionclass}] [VALIDATIONSTRING {valstr}] 
	                   TO PLUGINCAPTION {plugincaption} PLUGINID {pluginid}; 

For given action class, sends validation string to executable/non-executable plugins to check valid plugins for given parameters. If there is only one plugin, its caption and id are set to target parameters. If there are multiple plugins, a pop up appears to help user to select appropriate plugin for given action.

Action class is used to get related plugins for given action class. It is optional, if it is not given in validate command validation string is sent to all executable/non-executable plugins. Validate string is a business layer validation string. This parameters is send to related plugins to check business layer data, to check whether plugin is valid for given action type. 

Plugin caption is a target string symbol to get name for selected plugin caption and  pluginid is a target string symbol to get selected plugin id. This value can be used as target PLUGINACTION command

Plugin Class
=============

To access a plugin easily, PLUGINACCESS which is a wrapper class is included in standard code database. Basic methods of this class are below:

**VOID DOACTIONWP(STRING PACTIONCLASS, STRING PACTIONTYPE, STRING PACTIONVALUE) :**This method sends given action parameters to PluginService. If class has a target plugin information this action is sent to target plugin automatically. This method uses PLUGINACTION command.

**VOID SETDEFAULTACTIONCLASS(STRING PACTIONCLASS) :**Sets default action class, and uses this action class for all actions.

**VOID DOACTION(STRING PACTIONTYPE, STRING PACTIONVALUE) :**This method sends given action parameters to PluginService. Uses default action class which is set by SETDEFAULTACTIONCLASS() method. If class has a target plugin information this action is sent to target plugin automatically. This method uses PLUGINACTION command.

**STRING SELECTTARGET(STRING PACTIONCLASS, STRING PVALSTRING) :**Checks appropriate plugins using given parameters. If there are multiple applications which is valid for given parameters, shows selection dialog on client side. If there is only one plugin it sets target plugin information for this PLUGINACCESS instance. Returns target plugin’s id.

**VOID CLEARTARGET() :**Clears target plugin id and caption.

**STRING GETTARGET() :**Returns target plugin id. If there is not a target plugin returns empty string. To select a target you must call SELECTTARGET() method.

**STRING GETTARGETCAPTION() :**Returns target plugin caption.

**VOID CLEARTARGET() :** Clear target plugin id and caption for this instance.

Here is an example which sends a single message to a plugin:

::

	OBJECT: 
		 PLUGINACCESS PACCESS1;

	PACCESS1.DOACTIONWP('BITOOL','OPENANALYSIS','params');


Another example that sends multiple actions to a selected plugin:

::

	OBJECT: 
		PLUGINACCESS PACCESS1;

	PACCESS1.SETDEFAULTACTIONCLASS('BITOOL');

	PACCESS1.DOACTION('OPENANALYSIS1','params');
	PACCESS1.DOACTION('OPENANALYSIS2','params');
	PACCESS1.DOACTION('OPENANALYSIS3','params');
	

Another example that shows selecting a target plugin to send next messages directly :

::

	OBJECT: 
		PLUGINACCESS PACCESS1;

	PACCESS1.SELECTPLUGIN('BITOOL');
	
	PACCESS1.SETDEFAULTACTIONCLASS('BITOOL');
	PACCESS1.DOACTION('OPENANALYSIS1','params');
	PACCESS1.DOACTION('OPENANALYSIS2','params');










	