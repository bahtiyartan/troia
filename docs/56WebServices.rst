

============
Web Services
============

*TROIA Platform supports file transfer operations between application server and file server. This section aims to introduce ftp operations and related commands.*

Introduction
------------

..introduction


WSDL Overview
=============

Installation
------------

..installation

..Testing Web Service Installation

Defining TROIA Methods as Web Service
-------------------------------------

..defining

Web Service User Rights
=======================
.

Log-in/Log-out over Web Service
-------------------------------

..loginlogut

Listing Available Services
--------------------------

..listservices

Calling Services
----------------

..calling services


Encryption
----------

As its default behavior, system does not use encrypted communication. If encrypted communication is needed due to applications requirements, client applications must send true value as encryption information on login request.

Web Service encryption infrastructure uses AES as encryption standard (if required CipherMode:CBC, PaddingMode:PKCS7, KeySize:128 and BlockSize:128).  Required public key is supplied by application server and sent to client application on LoginResponse.EncryptionKey field. This value must be converted to byte array using UTF-8 encoding to get final encryption key for client side encryption and decryption processes. Encryption process converts string response to byte array using UTF-8 encoding and encrypts returning byte array. After encryption process resulting byte array is converted to Base64 String to enabling data transfer over web service. As a result of this process in order to get pure string response of web service call, client applications must convert Base64 String to byte array, decrypt this byte array and convert this byte array to string using UTF-8 encoding.

Additionally, for encrypted connections, client applications must send Parameters string as an encrypted string. The way of encryption must be same as server side encryption process and resulting value must be Base64 String. 

.. figure:: images/webservices/encryption.png
   :width: 700 px
   :target: images/webservices/encryption.png
   :align: center


Compression
-----------

If compression enabled and length of service’s string response is greater than minimum compress size(4000 characters), application server converts string data to byte array with UTF-8 encoding, compress byte array and creates Base64 String. If server makes compression over pure response string, Compress field of CaniasResponse is set to true. Thus, if Compress flag is set to true, client application must convert Base64 String to byte array, decompress and convert decompressed byte array to string with UTF-8 encoding. Application Server’s web service infrastructure uses Zip Stream (DEFAULT_STRATEGY) while compressing byte arrays.
System does not apply compression to encrypted data.

.. figure:: images/webservices/compression.png
   :width: 700 px
   :target: images/webservices/compression.png
   :align: center


