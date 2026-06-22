Frequently Asked Questions
==========================

Q. Can I use the LM with ntop applications running on Kubernets or Docker ?
---------------------------------------------------------------------------

Yes you can. Make sure that the LM is installed on a host whose syetemId does NOT change (e.g. a physical host or a VM). This is the only requirement. All other applications (e.g. ntopng and nProbe0 can be safely installed on containers and PODs moving across physical hosts.


Q. Can I install multiple license types on the same LM instance?
----------------------------------------------------------------

There is no limitation for license types. You can simultaneously install multiple license files in /etc/ntop/license_manager/ for different ntop products.


Q. How is the license validated with the LM?
--------------------------------------------

When validating the licenses with LM, ntop applications such as ntopng operate as follows

- The license is enforced at startup by contacting the LM.
- If the check is successfull the application starts up as usual, otherwise it terminates.
- Upon sucessfull license validation the application stays connected with the LM.


Q. What happens to applications if the LM is unreacheable?
----------------------------------------------------------

At startup, if the LM is unreacheable the application is unable to validate its license and terminates. If the LM is terminated or unreachable, while the application is running, the applications tries to reconnect and if unable, it will terminate.


Q. Can I stop the LM after applications have validated their license?
---------------------------------------------------------------------

No, as already explained, terminating the LM will cause applications to top working.

Q. Can I use standard licenses with the LM?
--------------------------------------------

No, the LM needs special licenses designed for the LM.
