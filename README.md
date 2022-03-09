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

