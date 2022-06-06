==========================
Domains
==========================

*Domains are reusable data definitions that store basic features of a specific control, table field or database column. In this section aims to introduce domains and its usage.*


What is Domain?
---------------

In large business applications, programmers have to use numerous text fields, table columns to store, read and show **same** kind of data for different table and forms. So they have to make same metadata definitions like length, formatters, description texts etc. in different forms and tables. For example, assume a textfield that stores/shows "customer name" information on an invoice document. Most probably, we must have same "customer name" fields in your related data tables and a "customer name" field in our printable invoice report. More importantly, there are tens of "customer name" field in your other dialogs, reports of your other transactions. All this fields have same metadata definitions to have identical behaviour and appearance.

In a large business application scenario; if we can not overcome such a problem it dramatically increases our development and maintenance cost. It is not easy to find and modify some features of all controls that handles "customer name" in whole application interface. Briefly, it is a must to deal with this hard problem for an enterprise level programmer to reduce development and maintenance costs.

It is very hard to manage such a problem in an efficient way. The most common way to overcome this problem is developing and using, reusable custom types, components. Even more, that is the only way for the programmers who the mostly used programming languages.


Domain Data
------------

The first type of domain data is mostly used for database fields and ui controls. All this information is same for all the database fields and controls that share same domain data. In other words, if you change one of these information, all fields and controls starts to use the new data in those related features after a simple convert/save and table convert operation in ODBA.

+-----------------------+-------------------------------------------------------------------------------------------------------------+
| Field Name            | It is just a guide for developers for naming table columns, it is not used by TROIA interpreter             |
+-----------------------+-------------------------------------------------------------------------------------------------------------+
| Field Type            | Stores data type ODBA or virtual table column that extend this domain.                                      |
+-----------------------+-------------------------------------------------------------------------------------------------------------+
| Field Length          | Stores field length for ODBA or virtual table column. (for only valid types like string (varchar))          |
+-----------------------+-------------------------------------------------------------------------------------------------------------+
| Field Decimal         | Stores number of decimal digits for ODBA columns that extend domain.                                        |
+-----------------------+-------------------------------------------------------------------------------------------------------------+
| Check Table           | Stores related zoom information for zooming operations.                                                     |
| Zoom Dialog           |                                                                                                             |
| Zoom Field            |                                                                                                             |
| Zoom Filter           |                                                                                                             |
+-----------------------+-------------------------------------------------------------------------------------------------------------+
| Read/Zoom Only        | Shows ui control or table cell is read only or zoom only.                                                   |
+-----------------------+-------------------------------------------------------------------------------------------------------------+
| Case Sensitivity      | Shows ui control or table cell is upper case/lower case.                                                    |
+-----------------------+-------------------------------------------------------------------------------------------------------------+
| Field Picture         | Stores required data for virtual table column information, for columns that extend domain.                  |
+-----------------------+-------------------------------------------------------------------------------------------------------------+
| Set-Get Id            | Stores setgetid, for textfields. (Enabling/disabling "set" and "get" are configured for each control.       |
+-----------------------+-------------------------------------------------------------------------------------------------------------+


All these information can be overridden by the programmer for specific controls,ODBA fields and virtual table fields. To use values defined in domain string values must be defined as {D} and integer values must be defined as -10, otherwise values given in control, odba table or virtual table overrides the value that comes from domain. These {D} and -10 values are special values that are also used while inheriting domains from some base domains. We will discuss it, in next titles. 


Domain Codes
------------

Regular Event Codes
=======================

It is also possible to define some regular event codes on domain instead of defining them for each control. So, when it is needed, if programmer change the code defined for domain, this change affect behaviour for all control events whose control extends the domain. 

Codes defined in domain affect all controls that don't have a corresponding event implementation on control level. This means that there is no need to add a {D} like wildcard. And also it means, it is possible to add alternative behaviors for controls that extend same domain, on different dialogs and forms and components.

TEXTCHANGED, ZOOMBEFORE, ZOOMAFTER, LOSEFOCUS, GAINFOCUS, RIGHTCLICKMENU, DRAG , DROP, PASTE are the events that programmers can define on a domain. For more up to date list of events for a domain, please see actual domain definition transaction that we will discuss in next sections.

Transfer Codes
=======================

+-----------------------+-----------------------------------+
| CLIENTTOSERVER CODE   |                                   |
+-----------------------+-----------------------------------+
| SERVERTOCLIENT CODE   |                                   |
+-----------------------+-----------------------------------+

Validation Code
=======================




Defining Domains
----------------


How are Domains Stored?
=======================


Using Domains
-------------


Domains in Dialog & Report Controls
===================================


Domains in Table Column Information
===================================


Domains and Security
--------------------

Is Personal
Data Masking Function
AccessControl       yxxx