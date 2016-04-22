

=========================
Client Plugin Development
=========================

*TROIA Platform offers an api to integrating third party applications with TROIA client. This sections aims to introduce client plugin api to developers who wants to develop applications integrated with client application of TROIA Platform.*


Introduction
------------

Plugins are third party applications or libraries that are able to communicate with client application over plugin service. It is possible to develop a wide range of plugins from simple logging applications to enterprise level business intelligence tools using java programming language.


Enabling Plugin
===============

To enable plugin check Menu -> Settings -> Plugin -> Enable Plugins checkbox.

Installation Details
====================

If plugin service is enabled, client deploys and starts Plugin Service automatically. Plugin Service is installed to {userhomefolder}\RESOURCES\PluginService\ folder. PluginService is also updated by Canias Client. Plugin Infrastructure is supported on 5.02.01 100801 and following versions.


Components of Plugin Infrastructure
===================================

**Plugin Service:** A standalone service that creates a communication infrastructure between client and plugins. This service is installed and run automatically if there is an open Canias client that allows plugin service (Canias-> Settings -> Plugin->Enable Plugins) 

**caniasplugin.jar:** a java library for developing all types of client plugins. This jar is supplied by IAS.

Types of Plugins
----------------

There are two types of client plugin. First one is executable plugin. Executable plugins are standalone and runnable applications that connect to Canias. This kind of applications must connect to plugin service using caniasplugin api. For example; business intelligence tools, drawing or cad tools.

Second type of plugins is non-executable plugin. This plugins are not runnable applications. They are installed to plugin service as library file. Plugin service fires (calls) their methods when a related action occurs. In this type of plugins there is no need an extra running application. For example: logging applications, etc.


How to Develop a Plugin
-----------------------

To connect a standalone application to TROIA Platform or develop a plugin "caniasplugin.jar" must be used as library. This api contains all required infrastructure communicate with client application. Using this api it is possible to send actions to Canias clients or listen actions from Canias Client. Two sample plugins(executable, non-executable) are supplied with this document.
 
Key Classes
===========

To develop a plugin, programmers must only inherit iasAbstractPlugin class which is supplied in caniasplugin.jar.

iasAbstractPlugin has abstract methods that must be overridden by your plugin. When and PLUGINACTION command run on an TROIA application, doAction(iasPluginAction p) method is fired. To send events to Canias Client from a plugin use postEvent(iasPluginEvent e, String target) method.

iasPluginAction class contains all required information such as action class, action type, action value, session id, transaction id, username and transaction. Event source is also contained by iasPluginAction. Using event source value, it is possible to send events to Canias Client which sends previous action.
To fire an action on Canias Client, plugins must send an iasPluginEvent using postEvent() method of iasAbstractPlugin. Event type and event value must be supplied by plugin. 

Details about all required classes are in Javadoc files which are supplied with this document and caniaspluging.jar. Also, sample plugins include usage examples of this classes.

Deploying a Plugin
==================

To deploy second type of plugin (non-executable) plugin jar must be copied into …\RESOURCES\PluginService\plugins\ folder. For non-executable plugins jar file must have same name with the class that inherits iasAbstractPlugin. For example; if com.samplecompany.sammpleplugin.java  inherits iasAbstractPlugin jar’s name must be com.samplecompany.sammpleplugin.jar (See sample plugins Non-Executable)

Executable plugins are standalone applications, so there is no need to any special deployment action.


Sources of a Simple Generic Plugin
----------------------------------

These example contains source codes of a simple plugin that prints each action to a textfield. Its just have three simple classes. Here is the first class of the application:

.. code-block:: java

    package com.ias.client.plugin.test;

	import java.awt.Dimension;

	import javax.swing.JFrame;

	public class SampleApp {

		public static void main(String[] args) {
			SamplePluginFrame tool = new SamplePluginFrame();
			tool.setSize(new Dimension(600, 750));
			tool.setLocationRelativeTo(null);
			tool.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
			tool.setVisible(true);
		}
	}
	
And second class which consists user interface of sample plugin:

.. code-block:: java

	package com.ias.client.plugin.test;

	import java.awt.BorderLayout;
	import java.awt.GridLayout;
	import java.awt.event.ActionEvent;
	import java.awt.event.ActionListener;

	import javax.swing.BorderFactory;
	import javax.swing.JButton;
	import javax.swing.JFrame;
	import javax.swing.JLabel;
	import javax.swing.JPanel;
	import javax.swing.JScrollPane;
	import javax.swing.JTextArea;
	import javax.swing.JTextField;

	import com.ias.client.plugin.iasPluginEvent;
	import com.ias.client.plugin.iasPluginException;
	import com.ias.client.plugin.iasPluginPrededefinedActionTypes;

	@SuppressWarnings("serial")
	public class SamplePluginFrame extends JFrame implements ActionListener {

		private SamplePlugin m_iPlugin;

		private String m_strSessionId;

		JTextArea jIncomingActions = new JTextArea();

		JButton btnConnect = new JButton("Connect");
		JButton btnDisconnect = new JButton("Disconnect");

		JTextField jTargetClient = new JTextField();
		JTextField jActionType = new JTextField();
		JTextField jActionValue = new JTextField();
		JTextField jTransactionId = new JTextField();
		JButton jPostAction = new JButton("Post Action");

		public SamplePluginFrame() {
			super();

			m_strSessionId = "Session-" + ((int) (Math.random() * 1000));

			this.setTitle("Sample Plugin Test (" + m_strSessionId + ")");

			btnConnect.setActionCommand("connect");
			btnConnect.addActionListener(this);

			btnDisconnect.setActionCommand("disconnect");
			btnDisconnect.addActionListener(this);
			btnDisconnect.setEnabled(false);

			JPanel jIncomingActionsPanel = new JPanel(new BorderLayout());

			JScrollPane iPane = new JScrollPane(jIncomingActions);

			jIncomingActionsPanel.add(iPane);
			jIncomingActionsPanel.add(new JLabel("Incoming Actions"), BorderLayout.NORTH);

			JPanel jMainPanel = new JPanel(new BorderLayout());
			jMainPanel.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));
			jMainPanel.add(jIncomingActionsPanel);

			JPanel iHeaderPanel = new JPanel(new GridLayout(1, 3));
			iHeaderPanel.add(btnConnect);
			iHeaderPanel.add(btnDisconnect);
			iHeaderPanel.setBorder(BorderFactory.createEmptyBorder(0, 0, 10, 0));

			jMainPanel.add(iHeaderPanel, BorderLayout.NORTH);

			//
			JPanel iSouthPanel = new JPanel(new BorderLayout());

			JLabel iFormHeader = new JLabel("Outgoing Action");

			iFormHeader.setBorder(BorderFactory.createEmptyBorder(10, 0, 10, 0));
			iSouthPanel.add(iFormHeader, BorderLayout.NORTH);

			JPanel iOutgoingActionForm = new JPanel(new GridLayout(5, 2, 0, 3));
			iOutgoingActionForm.add(new JLabel("TargetClient"));
			iOutgoingActionForm.add(jTargetClient);

			iOutgoingActionForm.add(new JLabel("ActionType"));
			iOutgoingActionForm.add(jActionType);

			iOutgoingActionForm.add(new JLabel("ActionValue"));
			iOutgoingActionForm.add(jActionValue);

			iOutgoingActionForm.add(new JLabel("TransactionId"));
			iOutgoingActionForm.add(jTransactionId);

			iOutgoingActionForm.add(new JLabel(""));
			iOutgoingActionForm.add(jPostAction);
			
			jPostAction.setActionCommand("postaction");
			jPostAction.addActionListener(this);

			iSouthPanel.add(iOutgoingActionForm, BorderLayout.CENTER);

			jMainPanel.add(iSouthPanel, BorderLayout.SOUTH);
			
			jActionType.setText(iasPluginPrededefinedActionTypes.CANIASLINK);
			jActionValue.setText("canias://IASSALHEAD?parameters=CLIENT=00$COMPANY=02$DOCTYPE=D5$DOCNUM=00000000&mode=1&section=MAINDLG");

			this.setContentPane(jMainPanel);

			enableDisablecomponents(false);
		}

		private void enableDisablecomponents(boolean isConnected) {
			btnConnect.setEnabled(!isConnected);
			btnDisconnect.setEnabled(isConnected);

			jTargetClient.setEnabled(isConnected);
			jActionType.setEnabled(isConnected);
			jActionValue.setEnabled(isConnected);
			jTransactionId.setEnabled(isConnected);
			jPostAction.setEnabled(isConnected);
		}

		@Override
		public void actionPerformed(ActionEvent arg0) {

			if (arg0.getActionCommand().equalsIgnoreCase("connect")) {

				if (m_iPlugin == null || !m_iPlugin.isConnected()) {

					try {

						/*************************
						 * START: Critical point
						 * 
						 * Create an instance and call establishConnection()
						 ************************/

						m_iPlugin = new SamplePlugin(this, m_strSessionId);
						m_iPlugin.connect();

						/*************************
						 * END: Critical point.
						 *************************/

						enableDisablecomponents(true);

					} catch (Exception e) {
						e.printStackTrace();
					}

				}
			} else if (arg0.getActionCommand().equalsIgnoreCase("disconnect")) {

				try {
					m_iPlugin.disconnect();
					m_iPlugin = null;

					enableDisablecomponents(false);

				} catch (Exception e) {
					e.printStackTrace();
				}

			} else if (arg0.getActionCommand().equalsIgnoreCase("postaction")) {

				String strActionType = jActionType.getText();
				String strActionValue = jActionValue.getText();
				String strTargetClient = jTargetClient.getText();
				
				iasPluginEvent iEvent = new iasPluginEvent(strActionType, strActionValue);
				
				try {
					m_iPlugin.postEvent(iEvent, strTargetClient);
				} catch (iasPluginException e) {
					e.printStackTrace();
				}
			}
		}

		public void handleAction(String p_strAction, String p_strSource) {
			jIncomingActions.setText(jIncomingActions.getText() + p_strAction + "\n");
			
			jTargetClient.setText(p_strSource);
		}

	}

And third and most important class that inherits **iasAbstracPlugin** class which is provided in **caniasplugin.jar** library:

.. code-block:: java

	package com.ias.client.plugin.test;

	import java.rmi.RemoteException;

	import com.ias.client.plugin.iasAbstractPlugin;
	import com.ias.client.plugin.iasPluginAction;
	import com.ias.client.plugin.iasPluginValidationParameters;

	@SuppressWarnings("serial")
	/**
	 * This smaple plugin passes all action parameters to Frame to print on a text field
	 */
	public class SamplePlugin extends iasAbstractPlugin {

		public SamplePluginFrame Frame;
		private String m_strUniqueInstanceKey;

		protected SamplePlugin(SamplePluginFrame pFrame, String p_strUniqueInstanceKey) throws RemoteException {
			super();

			Frame = pFrame;
			m_strUniqueInstanceKey = p_strUniqueInstanceKey;
		}

		/**
		 * This method is called when an 
		 * PLUGINACTION command runs on canias application server.
		 * 
		 * this demo plugin converts iasPluginAction to a string.
		 */
		@Override
		public boolean doAction(iasPluginAction p_iAction) {
			StringBuilder sb = new StringBuilder();

			sb.append("ActionClass: ").append(p_iAction.getActionClass()).append("\n");
			sb.append("ActionType: ").append(p_iAction.getActionType()).append("\n");
			sb.append("ActionValue: ").append(p_iAction.getActionValue()).append("\n");
			sb.append("Source: ").append(p_iAction.getSource()).append("\n");
			sb.append("SessionId: ").append(p_iAction.getSessionId()).append("\n");
			sb.append("TransactionId: ").append(p_iAction.getTransactionId()).append("\n");
			sb.append("Username: ").append(p_iAction.getUsername()).append("\n");
			sb.append("Transaction: ").append(p_iAction.getTransaction()).append("\n");

			Frame.handleAction(sb.toString(), p_iAction.getSource());

			return true;
		}

		/**
		 * This method is called when PLUGINVALIDATE command runs on canias appliaction server.
		 * PLUGINVALIDATE command sends all validation parameters to all plugins which contains data (language,database etc.)
		 * about canias session.
		 * 
		 *  After this parameters is checked by plugin, if given params is valid for plugin 
		 *  plugin must send true.
		 *  
		 *  If multiple plugins are available, a pop up message appears on canias client to allow user select target plugin for
		 *  given action.
		 */
		@Override
		protected boolean validatePlugin(iasPluginValidationParameters params) {
			System.out.println("return true/false after validation paramters checked");
			return true;
		}

		/**
		 * Canias plugin service sends only related actions to this plugin.
		 */
		@Override
		public String[] getRelatedIncomingActionClasses() {
			return new String[] { "INFOSUITE" };
		}

		/**
		 * 
		 */
		@Override
		public String getAppName() {
			return "InfoSuite - Business Analytics";
		}

		@Override
		protected void disconnecting() {
			Frame = null;
		}

		@Override
		public String getAppInstanceKey() {
			return m_strUniqueInstanceKey;
		}
	}



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

**VOID DOACTIONWP(STRING PACTIONCLASS, STRING PACTIONTYPE, STRING PACTIONVALUE) :** This method sends given action parameters to PluginService. If class has a target plugin information this action is sent to target plugin automatically. This method uses PLUGINACTION command.

**VOID SETDEFAULTACTIONCLASS(STRING PACTIONCLASS) :** Sets default action class, and uses this action class for all actions.

**VOID DOACTION(STRING PACTIONTYPE, STRING PACTIONVALUE) :** This method sends given action parameters to PluginService. Uses default action class which is set by SETDEFAULTACTIONCLASS() method. If class has a target plugin information this action is sent to target plugin automatically. This method uses PLUGINACTION command.

**STRING SELECTTARGET(STRING PACTIONCLASS, STRING PVALSTRING) :** Checks appropriate plugins using given parameters. If there are multiple applications which is valid for given parameters, shows selection dialog on client side. If there is only one plugin it sets target plugin information for this PLUGINACCESS instance. Returns target plugin’s id.

**VOID CLEARTARGET() :** Clears target plugin id and caption.

**STRING GETTARGET() :** Returns target plugin id. If there is not a target plugin returns empty string. To select a target you must call SELECTTARGET() method.

**STRING GETTARGETCAPTION() :** Returns target plugin caption.

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










	