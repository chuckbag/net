# cron

## Overview: 
For commands that need to be executed repeatedly (e.g., hourly, daily, or weekly), you can use the crontab command. The crontab command creates a crontab file containing commands and instructions for the cron daemon to execute. You can use the crontab command with the following options:
- `crontab -a filename` Install filename as your crontab file. On many systems, this command is executed simply as crontab filename (i.e., without the -a option).
- `crontab -e` Edit your crontab file, or create one if it doesn't already exist.
- `crontab -l` Display your crontab file.
- `crontab -r` Remove your crontab file.
- `crontab -v` Display the last time you edited your crontab file. (This option is available on only a few systems.)
- `crontab -u user` Used in conjunction with other options, this option allows you to modify or view the crontab file of user. When available, only administrators can use this option.

Each entry in a crontab file consists of six fields, specifying in the following order:
```
  minute(s) hour(s) day(s) month(s) weekday(s) command(s)
```

The fields are separated by spaces or tabs. The first five are integer patterns and the sixth is the command to execute. The following table briefly describes each of the fields:

| Field	| Value	| Description|
|--|--|--|
 minute	 |0-59|	 The exact minute that the command sequence executes
 hour	| 0-23	| The hour of the day that the command sequence executes
 day	| 1-31	| The day of the month that the command sequence executes 
 month	| 1-12|	 The month of the year that the command sequence executes
 weekday	| 0-6	| The day of the week that the command sequence executes (Sunday = 0, Monday = 1, Tuesday = 2, and so forth)
 command|	 Special|	 The complete sequence of commands to execute. The command string must conform to Bourne shell syntax. Commands, executables (such as scripts), or combinations are acceptable.


Each of the patterns from the first five fields may be either * (an asterisk), meaning all legal values, or a list of elements separated by commas. An element is either a number or an inclusive range, indicated by two numbers separated by a minus sign (e.g., 10-12). You can specify days with two fields: day of the month and day of the week. If you specify both of them as a list of elements, cron will observe both of them, for example:

Easy format: 
```cron
* * * * * command to be executed
- - - - -
| | | | |
| | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
| | | ------- Month (1 - 12)
| | --------- Day of month (1 - 31)
| ----------- Hour (0 - 23)
------------- Minute (0 - 59)
```

To list all your cron jobs, type the following command:
```bash
# crontab -l
# crontab -u username -l
```

Instead of the first five fields, you can use any one of eight special strings. It will not just save your time but it will improve readability.
| special string | Description | 
|--|--|
`@reboot`     |    Run once, at startup.
`@yearly`     |    Run once a year, “0 0 1 1 *”.
`@annually` |(same as @yearly)
`@monthly` | Run once a month, “0 0 1 * *”.
`@weekly`    |     Run once a week, “0 0 * * 0”.
`@daily`    |     Run once a day, “0 0 * * *”.
`@midnight` |(same as @daily)
`@hourly`   |      Run once an hour, “0 * * * *”.

## Examples: 
Edit your own crontab file via the command
```bash
$ crontab -e
```


The cron daemon would run the program myprogram in the mydir directory on the first and fifteenth of each month, as well as on every Monday. To specify days by only one field, the other field should be set to *, for example:
```
0 0 1,15 * 1 /mydir/myprogram
```

The program would run only on Mondays.
```
0 0 * * 1 /mydir/myprogram
```
Run a job every 5 minutes: 
```
*/5 * * * * /path/to/job
```

To run /path/to/command five minutes after midnight, every day, enter:
```
5 0 * * * /path/to/command
```

Run /path/to/script.sh at 2:15pm on the first of every month, enter:
```
15 14 1 * * /path/to/script.sh
```

Run /scripts/phpscript.php at 10 pm on weekdays, enter:
```
0 22 * * 1-5 /scripts/phpscript.php
```

Run /root/scripts/perl/perlscript.pl at 23 minutes after midnight, 2am, 4am …, everyday, enter:
```
23 0-23/2 * * * /root/scripts/perl/perlscript.pl
```

Run /path/to/unixcommand at 5 after 4 every Sunday, enter:
```
5 4 * * sun /path/to/unixcommand
```

## References: 
- [What are cron and crontab, and how do I use them?](https://kb.iu.edu/d/afiz): Indiana Uni, Jan 2018
- [How To Add Jobs To cron Under Linux or UNIX](https://www.cyberciti.biz/faq/how-do-i-add-jobs-to-cron-under-linux-or-unix-oses/): nix Craft, Sep 2019


