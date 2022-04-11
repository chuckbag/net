# enabling CGI

## Overview:

To get apache to run cgi scripts:

edit the apache config file
```bash
vim /etc/httpd/conf/httpd.conf
```

and enable the addhandlier statement for the cgi-scripts (remove the hash):
```
#
# AddHandler allows you to map certain file extensions to "handlers":
# actions unrelated to filetype. These can be either built into the server
# or added with the Action directive (see below)
#
# To use CGI scripts outside of ScriptAliased directories:
# (You will also need to add "ExecCGI" to the "Options" directive.)
#
AddHandler cgi-script .cgi
```

Then make sure you restart apache to enable the changes
```bash
service httpd restart
```

