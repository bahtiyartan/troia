

============
Web Services
============

*TROIA Platform supports defining web services/methods which allows 3rd party applications to access business applications/database over TROIA codes. This section aims to introduce TROIA Web Service infrastructure for 3rd party application developers.*

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

login() method
==============

login () method creates a connector session on CANIAS Application Server if login credentials is correct.

Client, Language, DBServer, DBName, ApplicationServer, Username are default login parameters. Password parameter can be passed as MD5 hash or pure password string, authentication sub system detects password format automatically.

In addition to these parameters login method gets a boolean Encrypted flag. Encrypted flag enables encrypted connection between client application and CANIAS Web Service.

Client application must be indicate whether compression will be enabled or not at callService() requests during the session. If client application sends true as compression parameter, server activates automatic compression subsystem.

LCheck parameter is used internally; client application must pass an empty string as LCheck parameter.

VKey parameter is used internally; client application must pass an empty string as VKey parameter.


login() method returns LoginResponse complex type which has members below:

 - **Success (Boolean) :** If login is successful, this field is set to true, otherwise false.
 - **SessionId (String) :** This member returns user’s session id, otherwise. If login fails it is an empty string.
 - **SecurityKey (String) :** Application Server returns a random security key for each successful login. Client application must pass this security key parameter while calling callService () method, to indicate it’s an authenticated application.
 - **ContactNum (String) :** This member returns users ContactNum which is stored in CONTACTNUM column of IASUSERS table.
 - **ErrorMessage (String) :** If login fails, this field returns login error message in given language; else this field is an empty string.
 - **EncryptionKey (String):** If client application connects an encrypted connection, application server returns an EncryptionKey which will be used at service interactions. Client application must convert EncryptionKey to byte array using UTF8 encoding before using this key as encryption key.





Listing Available Services
--------------------------

**listServices()** method of TROIA web service gets SessionId as string parameter and returns all available services as string array. **Web Services which user has not permission to call are not included in returning array.**
 
If returning array does not include name of web service that you want to call, you must check whether your method is registered as web service and user who is connected as web service  client has permission to run registered service.


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


