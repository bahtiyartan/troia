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
-----------

Field Type
Field Length
Field Decimal
Check Table
Zoom Dialog
Zoom Field
Read Only / Zoom Only
Case Sensitivity
Field Picture
Set-Get Id


Defining Domains
----------------


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
AccessControl