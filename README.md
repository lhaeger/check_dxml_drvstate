Novell DirXML 1.1 and Micro Focus (formerly NetIQ) Identity Manager 2.x/3.x/4.x driver state detector plugin for Nagios and Icinga - basically a wrapper for "dxcmd -getstate".


    Usage: check_dxml_drvstate [-s <server>] -u <username>, -p <password> -d <driver-dn> [-i] [--tw <warnsize> --tc <criticalsize> [--tree <treename>]]
    Usage: check_dxml_drvstate [-h | --help | -?]
    Novell DirXML and Novell/NetIQ Identity Manager driver state detector plugin for Nagios/Icinga
    Version 2.2, 2021-06-10
    
      -s, --server     DirXML/IDM server IP or hostname, e.g. 127.0.0.1 or myserver.mydomain.org.
                       Leave out this option to check drivers running on the same machine as nrpe.
          --edirport   eDirectory (NCP) port. Defaults to 524 if not specified.
          --ldapmode   TLS, SSL or CLEAR. Defaults to TLS (=StartTLS on LDAP cleartext port) if not specified.
          --ldapport   LDAP port. Defaults to 636 if not specified and LDAP mode is SSL (which 
                       includes TLS 1.0 and higher on secure LDAP port if your IDM version support it).
                       Defaults to 389 if not specified and LDAP mode is (Start)TLS or CLEAR.
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
          --perfdata   Append performance data to the output, so nagios can draw pretty graphs (e.g. <default output> | cache_age=42s;600;1800)
      -v, --verbose    Verbose output, -vv writes extra debug messages to /var/log/check_dxml_drvstate.log
      -l, --logfile    Logfile to write debug messages to instead of default
      -o, --overwrite  Overwrite log file on each run
      -h, -?, --help   This help screen

History:

    v1.0,  2006-04-10, initial release
    v1.1,  2007-05-21, added support for IDM 3.5 and more detailed return messages
    v1.2,  2007-07-31, added support for edir 8.8
                       new command line option "-i" to invert return codes of running and
                       stopped drivers. This is meant to help monitoring usually inactive
                       backup servers associated to a driver set.
                       all changes in v1.2 based on enhancements by Rainer Brunold, many thanks!
    v1.3,  2007-12-05, added TAO file size monitoring
                       username must now be ldap typed (for TAO file size monitoring)
                       take driver startup mode into consideration when driver not running:
                       disabled -> STATE_OK,
                       manual   -> STATE_WARNING
                       auto     -> STATE_CRITICAL
                       added long command line options
    v1.4,  2008-01-22, added heartbeat monitoring, requires a schema extension (aux class), driver
                        heartbeat and a special policy on the drive
                        new command line option --br to add html line breaks to text output
                        text output now shows warning/critical values for TAO file size and
                        heartbeat monitoring
    v1.5,  2008-08-26, added -Z parameter to ldapsearchs
                       improved TAO filesize determination for various "ls -l" output styles
    v1.6,  2008-09-01, fixed wrong $TAODIR for Edir 8.8x
    v1.7,  2009-03-12, added --nl parameter
                       minor bug fixes and cosmetics
    v1.6d, 2010-10-25, fixed TAO file finding logic for multi-instance eDirectory 8.8
                       changed ldapsearch calls from "-Z" to "-H ldaps://"
                       (David's branch)
    v1.7d, 2010-10-28, added optional port specifiers for eDirectory (524), LDAP (389), and
                       LDAPS (636) to allow non-default configurations to be monitored.
                       (David's branch)
    v1.8,  2010-11-10, merged David's and my branch
                       added --ldapmode, --ldapport and --edirport parameters based on David's
                       idea and original code
                       added -v, -vv and --verbose parameters, output is logged to /var/log/<scriptname>.log
                       added --short option
                       try to force use of openldap's ldapsearch to help avoid a bug in
                       Novell's ldapsearch implementation when using the -Z switch
                       minor bug fixes, code streamlining and trace cosmetics
    v1.9,  2010-11-21, rewrote the code to find edir tools and dib folder
                       added --bindir, --logfile, -l parameters
    v1.6j, 2012-04-26  Event Time checking added by <jplahl@novell.com>
                       (Joachim's branch)
    v2.0,  2012-07-29  merged Joachim's and my branch
                       added -o parameter to overwrite log file on each run
                       added --csw/--csc/--caw/--cac parameters
    v2.1,  2014-03-18  added --tjw/--tjc/--tjattr parameters
                       changed default heartbeat attr
                       added --ldaponly parameter (not yet implemented) 
    v2.1.1,2018-05-30  added --perfdata parameter for nagios performance data
                       (from Iwer Petersen's fork)
    v2.2,  2021-06-10  improved LDAP SSL/TLS default handling

Many thanks to David Gersic for adding multi-instance edir support, basic HA cluster support, custom LDAP/NDAP port parameters and more.

And to Joachim Plahl <jplahl@novell.com> for the original event time checking code and pointing my nose on using dxcmd stats to finally support remote cache age and size checks.

Thanks a lot to Iwer Peterson for adding support for Nagios performance data

Please report bugs to <lothar.haeger@is4it.de>. 


If you want to suppport this project [buy me a coffee](https://www.buymeacoffee.com/lhaeger)!
