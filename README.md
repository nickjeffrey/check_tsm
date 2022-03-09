# check_tsm
nagios check for IBM TSM / Spectrum Protect

# Requirements
perl, ssh, TSM credentials

# Configuration
This script is executed remotely on a monitored system by the NRPE or check_by_ssh methods available in nagios.

If you are using the check_by_ssh method, you will need a section in the services.cfg file on the nagios server that looks similar to the following. This assumes that you already have ssh key pairs configured.

    define service {
       use                             generic-service
       host_name                       tsmserv01
       service_description             TSM checks
       check_command                   check_by_ssh!/usr/local/nagios/libexec/check_tsm
       }

Alternatively, if you are using the NRPE method, you should have a section similar to the following in the services.cfg file:

    define service{
       use                             generic-service
       host_name                       tsmserv01
       service_description             TSM checks
       check_command                   check_nrpe!check_tsm -t 30
       notification_options            w,c,r                    ; Send notifications about warn, critical, and recovery events
       }

If you are using the NRPE method, you will also need a command definition similar to the following on each monitored host in the /usr/local/nagios/nrpe/nrpe.cfg file:

    command[check_tsm]=/usr/local/nagios/libexec/check_tsm


# Sample output
TSM Checks OK - TSM log util:1.7% / TSM db util:33.4% / TSM db buffer pool hit ratio:93.4% / drives online:5 / paths online:6 / scratch tapes:1 / client schedules completed=9 failed=0 missed=0 / admin schedules completed=12 failed=0 missed=0 / unavailable tapes:0 / readonly tapes:0 / Total slots in tape library:206 Used:134 Free:72

TSM checks CRITICAL - There are 24 missed TSM client schedules. Please check with query event * * TSM log util:1.7% / TSM db util:33.4% / TSM db buffer pool hit ratio:93.4% / drives online:5 / paths online:6 / scratch tapes:1 / client schedules completed=9 failed=0 missed=24 / admin schedules completed=12 failed=1 missed=0 / unavailable tapes:1 / readonly tapes:0 / Total slots in tape library:206 Used:134 Free:72 / Cleaning tapes:unknown Cleaning cycles left on tapes:0 TSM checks WARN Media fault dectected. This may be due to a bad tape or bad tape drive. Review the TSM actlog to see if you can find a tape with errors, then move the data off that tape. The actlog entry is: 03/08/22 09:32:27 ANR8359E Media fault detected on LTO volume TA0246L8 in drive DRIVE_5 (/dev/rmt3) of library LTOLIB1. (SESSION: 3294, PROCESS: 807) 
