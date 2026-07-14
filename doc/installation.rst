Installation and Configuration
==============================

LM is distributed as binary package names ```ntop-license``` and can be installed from `packages.ntop.org <https://packages.ntop.org>`_ as any other ntop package.

The package installs the ```ntop-license-manager``` binary LM application as a service

Windows
-------

At the moment, Windows systems are not supported by the LM.


Configuration
-------------

The LM configuration files are stored in /usr/share/ntop/etc/. Namely

- license-manager.conf is the LM configuration file
- cert.pem and key.pem as the files used by the LM to setup the TLS communications used by the applications to talk with the LM

The license files managed by the LM are instead installed in /etc/ntop/license_manager/ as individual files. Example: /etc/ntop/license_manager/nprobe.license.enterprise_m


LM Configuration File
~~~~~~~~~~~~~~~~~~~~~

The LM configuration file (/usr/share/ntop/etc/license-manager.conf) has the following format:

.. code:: text

   #
   # Listening IP:port (default: 0.0.0.0:7153)
   #
   --listen=0.0.0.0:7153
   
   #
   # Web listening IP:port (Default: 127.0.0.1:4444)
   #
   --web=0.0.0.0:4444
   
   #
   # User authentication tokens file (optional, no authentication by default)
   #
   #--auth-tokens=/usr/share/ntop/etc/auth_tokens.conf
   
   #
   # License leases log file (log all licenses allocated/released)
   #
   --lease-log=/var/log/ntop-license-manager-leases.log
   
   #
   # Verbose logs (debug)
   #
   --verbose

Where:

 - The user authentication tokens file is a file contains authentication tokens, one per line, used to authenticate the applications (see AUTH_TOKEN in the application configuration file below)
 - The license leases log file is used to dump the history of licenses allocated for the applications and when they are freed. 

Sample user authentication tokens file (e.g. whose path can be /usr/share/ntop/etc/auth_tokens.conf) containing 3 tokens:

.. code:: text

   OKR8OTKWZX1K1RPWKW9B3CLYYJGPA1Q9EECDDE011E4GUX4UEWKJY5YEEK1Y55TB
   Z6L0F0LS1O3ZK5PZBMEW0AFTDUK4V8AFZ7JW61G70XU39OOVIGLRPDZW8K9DUBE1
   3FDEP7U38LCPGBSRY3KVMDRMLM2LFWKO9T7EY4VYSSV75JP1B25V2QYZVT7IH1JF

TLS Configuration
~~~~~~~~~~~~~~~~~

Applications talk with the LM over TLS. The LM package comes with a self-signed certificate. You can use your own CA for generating the licenses or you can generate them with OpenSSL as follows

- openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 365

  
Starting/Stopping the Service
-----------------------------

The LM is managed as a standard Linux service as follows:

- service ntop-license-manager start
- service ntop-license-manager stop
- service ntop-license-manager restart

All commands need to be executed as superuser (root).


nProbe and ntopng Configuration for Using the LM
------------------------------------------------

nProbe and ntopng are currently the only applications that can use the LM. Such applications validate their licenses with the LM by specifying ```--license-mgr <conf file>```.

This configuration file (example under /usr/share/ntop/etc/license-manager-client.conf) has the following format:

.. code:: text

	  #
	  # IP and port of the license manager
	  #
	  LICENSE_MANAGER=127.0.0.1:7153

	  #
	  # Unique instance name
	  #
	  INSTANCE_NAME=example

	  #
	  # User authentication token (optional)
	  #
	  AUTH_TOKEN=OKR8OTKWZX1K1RPWKW9B3CLYYJGPA1Q9EECDDE011E4GUX4UEWKJY5YEEK1Y55TB

	  #
	  # nprobe ntopng
	  #
	  PRODUCT_FAMILY=nprobe

	  #
	  # pro enterprise_s enterprise_m enterprise_l enterprise_xl
	  #
	  PRODUCT_EDITION=enterprise_m

Where:

 - LICENSE_MANAGER is the IP and port of the host where the LM is running. If you want you can define a secondary license manager by adding a secondary address and port. Example: LICENSE_MANAGER=127.0.0.1:7153,192.168.1.23:7728. Note that if you use two license manager instances installed on two different hosts, you need two different set of licenses.
 - INSTANCE_NAME is a string used to indetity this specific instance
 - AUTH_TOKEN is a token that the LM can use to prevent issuing valid licenses for unknown AUTH_TOKEN. Its value must be configured in the LM.
 - PRODUCT_FAMILY and PRODUCT_EDITION define what license the ntop application will as the LM when contacting it.

Accepted PRODUCT_FAMILY values are ```nprobe```, ```ntopng```, ```n2disk```, ```cento```.

Accepted PRODUCT_EDITION values per product:

 - nprobe: ```pro```, ```enterprise_s```, ```enterprise_m```, ```enterprise_l```, ```enterprise_xl```
 - ntopng: ```pro```, ```enterprise_m```, ```enterprise_l```, ```enterprise_xl```, ```enterprise_xxl```
 - n2disk: ```1g```, ```5g```, ```19g```
 - cento: ```s```, ```m```, ```l```, ```xl```, ```xxl```
   
For more information about using the LM with ntop applications see:

 - `ntopng <https://www.ntop.org/guides/ntopng/versions_and_licensing.html#using-the-license-manager>`_
 - `nProbe <https://www.ntop.org/guides/nprobe/introduction.html#using-the-license-manager>`_
