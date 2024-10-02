# SELinux

## Turn on/off:
temporarily turn off:
```bash
echo 0 >/selinux/enforce
```

turn back on:
```bash
echo 1 >/selinux/enforce
```

## Alerting:
A number of SELinux alerts are suppressed by design. Unfortunately, on occasion we need to see these alerts to troubleshoot our applications.

On CentOS we can enable complete auditing by running:
```bash
semodule -b /usr/share/selinux/targeted/enableaudit.pp
```

When you no loner need complete auditing you should restore the system to its previous state to prevent the audit.log from filling up with unnecessary alerts:
```bash
semodule -b /usr/share/selinux/targeted/base.pp
```

See http://danwalsh.livejournal.com/11673.html for details.
