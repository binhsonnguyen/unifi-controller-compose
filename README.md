## Adopting Access Points and Unifi Devices

For your Unifi devices to "find" the Unifi Controller running in Docker, you MUST override the Inform Host IP with the address of the Docker host computer. (By default, the Docker container usually gets the internal address `172.17.x.x` while Unifi devices connect to the (external) address of the Docker host.) To do this:

Find Settings -> System -> Other Configuration -> Override Inform Host: in the Unifi Controller web GUI. (It's near the bottom of that page.)
Check the "Enable" box, and enter the IP address of the Docker host machine.
Save settings in Unifi Controller
Restart UniFi-in-Docker container with docker stop ... and docker run ... commands.
See Side Projects for other techniques to get Unifi devices to adopt your new Unifi Controller.

### Environment Variables

* UNIFI_HTTP_PORT This is the HTTP port used by the Web interface. Browsers will be redirected to the UNIFI_HTTPS_PORT. Default: 8080
* UNIFI_HTTPS_PORT This is the HTTPS port used by the Web interface. Default: 8443
* PORTAL_HTTP_PORT Port used for HTTP portal redirection. Default: 80
* PORTAL_HTTPS_PORT Port used for HTTPS portal redirection. Default: 8843
* UNIFI_STDOUT Controller outputs logs to stdout in addition to server.log Default: unset
* TZ TimeZone. (i.e America/Chicago)
* JVM_MAX_THREAD_STACK_SIZE Used to set max thread stack size for the JVM. Example: JVM_MAX_THREAD_STACK_SIZE=1280k, as a fix for https://community.ubnt.com/t5/UniFi-Routing-Switching/IMPORTANT-Debian-Ubuntu-users-MUST-READ-Updated-06-21/m-p/1968251#M48264⁠
* LOTSOFDEVICES Enable this with true if you run a system with a lot of devices and/or with a low powered system (like a Raspberry Pi). This makes a few adjustments to try and improve performance:
  * enable unifi.G1GC.enabled
  * set unifi.xms to JVM_INIT_HEAP_SIZE
  * set unifi.xmx to JVM_MAX_HEAP_SIZE
  * enable unifi.db.nojournal
  * set unifi.dg.extraargs to --quiet
  * See the [Unifi support site](https://help.ui.com/hc/en-us/articles/115005159588-UniFi-Tuning-the-Network-Application-for-a-High-Number-of-UniFi-Devices) for an explanation of some of those options. Default: unset
* JVM_EXTRA_OPTS Used to start the JVM with additional arguments. Default: unset
* JVM_INIT_HEAP_SIZE Set the starting size of the javascript engine for example: 1024M Default: unset
* JVM_MAX_HEAP_SIZE Java Virtual Machine (JVM) allocates available memory. For larger installations a larger value is recommended. For memory constrained system this value can be lowered. Default: 1024M

### Exposed Ports

* 8080/tcp - Device command/control
* 8443/tcp - Web interface + API
* 3478/udp - STUN service
* 8843/tcp - HTTPS portal (optional)
* 8880/tcp - HTTP portal (optional)
* 6789/tcp - Speed Test (unifi5 only) (optional)
* See UniFi - Ports Used⁠ for more information.

