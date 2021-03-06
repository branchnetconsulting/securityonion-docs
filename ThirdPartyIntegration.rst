Integrating with other systems
==============================

Many organizations would like to take data from Security Onion and send
it to third party systems.

Support
-------

We don't provide free support for third party systems, so this wiki page
will be just a brief introduction to how you would accomplish this. If
you need commercial support, please see:
https://www.securityonionsolutions.com

How do I send Bro and OSSEC logs to an external syslog collector?
-----------------------------------------------------------------

Configure ``/etc/syslog-ng/syslog-ng.conf`` with a new ``destination``
to forward to your external syslog collector and then restart
``syslog-ng``.

How do I send IDS alerts to an external system?
-----------------------------------------------

2 options:

-  Edit ALL ``/etc/nsm/HOSTNAME-INTERFACE/barnyard2*.conf`` files on ALL
   sensors with a new ``output`` to send IDS alerts to your external
   systems and then restart all barnyard2 instances:

   ::

       sudo so-barnyard-restart

OR

-  | On your master server (running sguild), configure
     ``/etc/syslog-ng/syslog-ng.conf`` with a new ``source`` to monitor
     ``/var/log/nsm/securityonion/sguild.log`` for ``Alert Received``
     lines and a new ``destination`` to send to your external system,
     and then restart ``syslog-ng``. To do this modify
     ``/etc/syslog-ng/syslog-ng.conf`` and add the following lines:
   | \`\`\`

   .. rubric:: This line specifies where the sguild.log file is located,
      and informs syslog-ng to tail the file, the program\_override
      inserts the string sguil\_alert into the string
      :name: this-line-specifies-where-the-sguild.log-file-is-located-and-informs-syslog-ng-to-tail-the-file-the-program_override-inserts-the-string-sguil_alert-into-the-string

   source s\_sguil { file("/var/log/nsm/securityonion/sguild.log"
   program\_override("sguil\_alert")); };

This line filters on the string “Alert Received”

filter f\_sguil { match("Alert Received"); };

This line tells syslog-ng to send the data read to the IP address of 10.80.4.37, via UDP to port 514

destination d\_sguil\_udp { udp("10.80.4.37" port(514)); };

This log section tells syslog-ng how to structure the previous ‘source / filter / destination’ and is what actually puts them into play

| log {
| source(s\_sguil);
| filter(f\_sguil);
| destination(d\_sguil\_udp);
| };
| \`\`\`

Please note that this option requires ``set DEBUG 2`` in ``/etc/sguild/sguild.conf``.
