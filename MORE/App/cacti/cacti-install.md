# cacti install on CentOS
- [cacti install on CentOS](#cacti-install-on-centos)
  - [Overview](#overview)
  - [Downloading and Installing](#downloading-and-installing)
    - [Downloading:](#downloading)
    - [Configure the DB:](#configure-the-db)
      - [Login to DB, set password:](#login-to-db-set-password)
      - [Secure DB:](#secure-db)
      - [Create Cacti DB:](#create-cacti-db)
    - [Configure Apache:](#configure-apache)
    - [Configure IPTables:](#configure-iptables)
  - [Configuring the Cacti Install](#configuring-the-cacti-install)
  - [Connect to the Cacti via the Web:](#connect-to-the-cacti-via-the-web)
  - [References:](#references)
    - [File Locations:](#file-locations)
    - [External Reference Material:](#external-reference-material)

## Overview
The following process outlines the steps needed to follow to install cacti on a CentOS server.  

## Downloading and Installing

### Downloading: 
Install the base applications required: 
```bash
yum install mysql-server mysql php-mysql php-pear php-common php-gd php-devel php php-mbstring php-cli php-snmp php-pear-Net-SMTP php-mysql httpd cacti
```

### Configure the DB: 

#### Login to DB, set password: 
First time you run mysql, the root password is not defined, so you can start it with this command. (`-u root` says user is root, and mysql says open the "mysql database")
```bash
mysql -u root mysql
```

If you already have a root password defined, then you will need to add the `-p` flag to be queried a password after the command is entered
```bash
mysql -u root mysql -p
```

Note that if you can also [reset the root password]() if needed.

Once in, set the root password:
```sql
SET PASSWORD FOR root@localhost = PASSWORD('DSxdWXdyCBB7');
quit
```

#### Secure DB: 
You want to make sure that the db is setup properly, and that any unneeded initial tables are not included.  In CentOS this is really simple, just run the following command and follow the instructions.  
```bash
/usr/bin/mysql_secure_installation
```

See [detailed instructions]() on using this command if needed. 

#### Create Cacti DB:
Create the cacti instance: 
```bash
mysqladmin -u root -p create cacti
```

At password prompt, enter in DSxdWXdyCBB7

then provide it a structure: 
```bash
mysql -p cacti < /usr/share/doc/cacti-0.8.7g/cacti.sql
```
At password prompt, enter in DSxdWXdyCBB7

Create a cacti user: 
```bash
mysql -u root -p mysql
```

At password prompt, enter in DSxdWXdyCBB7

then at the mysql prompt, enter in the following: 
```sql
GRANT ALL ON cacti.* TO cacti@localhost IDENTIFIED BY '9Bc6Qw4YqXyU';
flush privileges; 
exit
```

### Configure Apache: 
edit the httpd.conf file: 
```bash
vim /etc/httpd/conf/httpd.conf
```

and hard code in the server name to prevent any errors at startup: 
```
#ServerName www.example.com:80
ServerName cacti.cmed.us:80
```

modify the cacti.conf file: 
```bash
vim /etc/httpd/conf.d/cacti.conf
```

And allow access to the /cacti server.  You can do this by allowing only specific networks: (10.50.0.0/16 and 10.33.0.0/16)  See in red. 
```
<Directory /usr/share/cacti/>
        Order Deny,Allow
        Deny from all
        Allow from 127.0.0.1 10.50 10.33
</Directory>
```

Or you can allow access from all IP's
```
<Directory /usr/share/cacti/>
        Order Deny,Allow
        Allow from all
</Directory>
```
and restart apache to take changes: 
```bash
service httpd restart
```

### Configure IPTables: 

view the current rules setup: 
```bash
iptables-save
```

modify the iptables config
```bash
vim /etc/sysconfig/iptables
```

and allow port 80 access inbound: 
```bash
-A RH-Firewall-1-INPUT -m state --state NEW -p tcp --dport 80 -j ACCEPT
```

then restart iptables: 
```bash
service iptables restart
```

## Configuring the Cacti Install
Tell Cacti how to get to the db by modifying the following file: 
```bash
vim /usr/share/cacti/include/config.php
```

Update the username and password: 
```
$database_username = "cacti";
$database_password = "9Bc6Qw4YqXyU";
```

Create a cacti user on the linux box, and setup a cronjob to run: 
```bash
adduser cacti
chown -R cacti /usr/share/cacti/rra
chown -R cacti /usr/share/cacti/log
chown -R cacti /var/lib/cacti/rra/
chown -R cacti /var/log/cacti/ 
```

(the last two might not be needed, just check the first two to make sure they are/are not symlinked, and make sure the symlinks have their files modified too. )

then create the cronjob: 
```bash
vim /etc/cron.d/cacti
```

and add the following line: 
```cron
*/5 * * * * cacti /usr/bin/php /usr/share/cacti/poller.php > /dev/null 2>&1
```

## Connect to the Cacti via the Web: 
from the browser, connect to: 
- `http://{server}/cacti`

and then reference the next document: Configuring Cacti.


## References: 
### File Locations: 

| Descriptor | location |
|--|--|
Path for cacti:   |      `/usr/share/cacti/`
Path for httpd.conf:  | `/etc/httpd/conf/httpd.conf`
Path for cacti.conf: | `/etc/httpd/conf.d/cacti.conf`   

### External Reference Material: 
- [Cacti 0.8](http://my.safaribooksonline.com/book/-/9781849513920), Thomas Urban, March, 2011, Print ISBN-13: 978-1-84951-392-0
  