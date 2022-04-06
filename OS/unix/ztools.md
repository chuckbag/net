# zTools

## zcat:
cat'ing compressed files:
```bash
chuck /var/log $ zcat syslog.2.gz | more
Jun 11 09:02:19 oak rsyslogd: [origin software="rsyslogd" swVersion="4.2.0" x-pid="774" x-info="
http://www.rsyslog.com"] rsyslogd was HUPed, type 'lightweight'.
Jun 11 09:02:20 oak anacron[28954]: Job `cron.daily' terminated
Jun 11 09:02:20 oak anacron[28954]: Normal exit (1 job run)
Jun 11 09:03:09 oak Synergy 1.3.1: WARNING: synergyc.cpp,265: failed to connect to server: No route to host
```

## zgrep:
grepping compressed files: (note that this includes grep variables (like "`-v`"))
```bash
chuck /var/log $ zgrep -v synergyc cron syslog.2.gz | grep -v CRON
gzip: cron.gz: No such file or directory
syslog.2.gz:Jun 11 09:02:19 oak rsyslogd: [origin software="rsyslogd" swVersion="4.2.0" x-pid="774" x-info="http://www.rsyslog.com"] rsyslogd was HUPed, type 'lightweight'.
syslog.2.gz:Jun 11 09:02:20 oak anacron[28954]: Job `cron.daily' terminated
syslog.2.gz:Jun 11 09:02:20 oak anacron[28954]: Normal exit (1 job run)
syslog.2.gz:Jun 11 09:27:42 oak ntpd[1558]: kernel time sync status change 2001
```

## zsync:



## Ref:
- [the power of Z commands](http://www.thegeekstuff.com/2009/05/zcat-zless-zgrep-zdiff-zcmp-zmore-gzip-file-operations-on-the-compressed-files/#more-463):Zcat, Zless, Zgrep, Zdiff Examples
- [zsync file transfer program:](http://zsync.moria.org.uk/) rsync with compression

