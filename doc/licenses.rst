Managing Licenses
=================

The ntop license manager allows you to manage licenses for ntop products like nprobe and ntopng.
This section explains how to add and configure new licenses.

Purchasing a License
--------------------

The first step is to purchase a license from https://shop.ntop.org. Once purchased, you will 
receive an order ID which is needed for license activation.

Generate New License Key
------------------------

To generate a license key, you will need:

- System ID (obtained via ```ntop-license-manager -v```)
- Order ID (received after purchase)
- Email address used for purchase
- Product version (from ```nprobe --version``` or ```ntopng --version```, e.g. 10.2.240123)

Adding a New License
--------------------

Licenses should be placed under ```/etc/ntop/license_manager/```.
When starting up the license manager, it scans the folder and load all valid licenses.

If you need to add new licenses, while the license manager is running:

1. Create a new file under /etc/ntop/license_manager/ with a new unique name
2. Add your license key to this file
3. Hot reload the license manager configuration with the following command:

.. code-block:: bash
		
   systemctl kill -s HUP ntop-license-manager

Verify the reload by checking the logs:

.. code-block:: bash
		
   journalctl -u ntop-license-manager -f

You should see output similar to:

.. code-block:: none
		
   server systemd[1]: ntop-license-manager.service: Sent signal SIGHUP to main process 2401413 (ntop-license-ma) on client request.
   server ntop-license-manager[2401413]: 31/Jan/2025 15:36:11 [Manager.cpp:25] Reloading licenses...
   server ntop-license-manager[2401413]: 31/Jan/2025 15:36:11 [Manager.cpp:132] Scanning license directory /etc/ntop/license_manager
   server ntop-license-manager[2401413]: 31/Jan/2025 15:36:11 [Manager.cpp:205] [VALID] License files verified successfully

The log output will confirm that your licenses have been loaded and validated successfully.
