Novell DirXML 1.1 and Identity Manager 2.x/3.x/4.x driver state detector 
plugin for Nagios. Basically a wrapper for "dxcmd -getstate". 


    Usage: check_dxml_drvstate [-s <server>] -u <username>, -p <password> -d <driver-dn> [-i] [--tw <warnsize> --tc <criticalsize> [--tree <treename>]]
    Usage: check_dxml_drvstate [-h | --help | -?]
    Novell DirXML and Novell/NetIQ Identity Manager driver state detector plugin for Nagios/Icinga
    Version 2.1, 2014-03-18
    
      -s, --server     DirXML/IDM server IP or hostname, e.g. 127.0.0.1 or myserver.mydomain.org.
                       Leave out this option to check drivers running on the same machine as nrpe.
          --edirport   eDirectory (NCP) port. Defaults to 524 if not specified.
          --ldapmode   TLS, SSL or CLEAR. Defaults to TLS if not specified
          --ldapport   LDAP port. Defaults to 636 if not specified and LDAP mode is SSL.
                       Defaults to 389 if not specified and LDAP mode is TLS or CLEAR.
      -u, --username   Account used to check driver state, ldap typed syntax, e.g. cn=admin,o=novell
      -p, --password   Password in cleartext (good reason to use a restriced account :-)
      -d, --driver     Driver to check, ldap typed syntax, cn=drv_test,cn=my_driverset,o=system
      -i, --invert     Invert return codes to monitor inactive backup servers in a driverset.
                       A running driver will return STATE_CRITICAL (2), a stopped one STATE_OK (0)
          --tw         Max TAO file size before STATE_WARNING (1) will be reported
          --tc         Max TAO file size before STATE_CRITICAL (2) will be reported
                       If neither --tw and --tc are set, TAO file size checking will be disabled
                       (--tw/--tc parameters are deprecated: use --csw/--csc instead)
          --csw        Max cache size before STATE_WARNING (1) will be reported
          --csc        Max cache size before STATE_CRITICAL (2) will be reported
                       If neither --csw and --csc are set, cache size checking will be disabled
                       (--csw/--csc parameters replace --tw/--tc; cache size is TAO file size minus 72 bytes)
          --caw        Max cache age (in seconds) before STATE_WARNING (1) will be reported
          --cac        Max cache age (in seconds) before STATE_CRITICAL (2) will be reported
                       If neither --caw and --cac are set, cache size checking will be disabled
          --hbw        Max time in seconds since last publisher heartbeat before STATE_WARNING (1) will be reported
          --hbc        Max time in seconds since last publisher heartbeat before STATE_CRITICAL (2) will be reported
                       If neither --hbw and --hbc are set, publisher heartbeat checking will be disabled
                       Please note that a schema extension and a special publisher event transform policy on the
                       driver are necessary to support heartbeat checking
          --hbattr     LDAP name of the attr that stores the last publisher heartbeat timestamp if a non-default
                       schema extension is used
          --tjw        Max time in seconds since last trigger job before STATE_WARNING (1) will be reported
          --tjc        Max time in seconds since last trigger job before STATE_CRITICAL (2) will be reported
                       If neither --tjw and --tjc are set, trigger job checking will be disabled
                       Please note that a schema extension and a special subscriber event transform policy on the
                       driver are necessary to support trigger job checking
          --tjattr     LDAP name of the attr that stores the last trigger job timestamp if a non-default
                       schema extension is used
          --tree       Treename of the driver to be checked. Only needed with TAO file size monitoring
                       on edir 8.8 running multiple instances. If not set, the first instance reported
                       by ndsconfig get will be used
          --short      print short output, omit driver and file names
          --br         Add <br> tags to output for better readability in HTML display
          --nl         Add line breaks to output for better readability in console/file output
          --bindir     Directory where dxcmd and ndsconfig binaries are located
      -v, --verbose    Verbose output, -vv writes extra debug messages to /var/log/check_dxml_drvstate.log
      -l, --logfile    Logfile to write debug messages to instead of default
      -o, --overwrite  Overwrite log file on each run
      -h, -?, --help   This help screen
    
Many thanks to David Gersic for adding multi-instance edir support, basic HA cluster support, custom LDAP/NDAP port parameters and more.

And to Joachim Plahl <jplahl@novell.com> for the original event time checking code and pointing my nose on using dxcmd stats to finally support remote cache age and size checks.

Please report bugs to <lothar.haeger@is4it.de>.