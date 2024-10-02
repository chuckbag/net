# instaling CPAN
This is all well documented elsewhere.  For complete (and correct notes, see References below.)

## Installing CPAN:
Run CPAN shell:
```
perl -MCPAN -e shell
```
This will start up the auto-setup app within cpan. The questions are very simple, and are outputted below for basic reference. Note that in the section where it asks: "Where is your X program?" you might need to make sure that you have all of those apps installed. If you have wget and links, you can use that instead of ftp, and that's why I left those blank.
```
[...]
Are you ready for manual configuration? [yes] yes
[...]
CPAN build and cache directory? [/root/.cpan]
[...]
Cache size for build directory (in MB)? [10]
[...]
Perform cache scanning (atstart or never)? [atstart]
[...]
Cache metadata (yes/no)? [yes]
[...]
Your terminal expects ISO-8859-1 (yes/no)? [yes]
[...]
File to save your history? [/root/.cpan/histfile]
[...]
Number of lines to save? [100]
[...]
Policy on building prerequisites (follow, ask or ignore)? [ask]
[...]
Where is your gzip program? [/bin/gzip]
Where is your tar program? [/bin/tar]
Where is your unzip program? [/usr/bin/unzip]
Where is your make program? [/usr/bin/make]
Where is your links program? [/usr/bin/links]
Where is your wget program? [/usr/bin/wget]
Warning: ncftpget not found in PATH
Where is your ncftpget program? []
Warning: ncftp not found in PATH
Where is your ncftp program? []
Where is your ftp program? [/usr/kerberos/bin/ftp]
Where is your gpg program? [/usr/bin/gpg]
What is your favorite pager program? [/usr/bin/less]
What is your favorite shell? [/bin/bash]
[...]
    PREFIX=~/perl       non-root users (please see manual for more hints)
Your choice:  []
[...]
    -j3              dual processor system
Your choice:  []
[...]
    UNINST=1         to always uninstall potentially conflicting files
Your choice:  []
[...]
Timeout for inactivity during Makefile.PL? [0]
[...]
Your ftp_proxy?
Your http_proxy?
Your no_proxy?
[...]
Select your continent (or several nearby continents) [] 6
Select your country (or several nearby countries) [] 4
put them on one line, separated by blanks, e.g. '1 4 5' [] 1 10

Enter another URL or RETURN to quit: []
New set of picks:
  ftp://cpan-du.viaverio.com/pub/CPAN/
  ftp://ftp-mirror.internap.com/pub/CPAN/


commit: wrote /usr/lib/perl5/5.8.8/CPAN/Config.pm
Terminal does not support AddHistory.

cpan shell -- CPAN exploration and modules installation (v1.7602)
ReadLine support available (try 'install Bundle::CPAN')

cpan>
```

## Installing Modules within CPAN:
To get back into the CPAN console, simply reenter the same command that you used to originally install the app: 
```
perl -MCPAN -e shell
```
then to install a module,
```
cpan> install use Getopt::Long;
CPAN: Storable loaded ok
CPAN: LWP::UserAgent loaded ok
Fetching with LWP:
  ftp://cpan-du.viaverio.com/pub/CPAN/authors/01mailrc.txt.gz
Going to read /root/.cpan/sources/authors/01mailrc.txt.gz

[...]

Running make install
Installing /usr/lib/perl5/5.8.8/newgetopt.pl
Installing /usr/lib/perl5/5.8.8/Getopt/Long.pm
Installing /usr/share/man/man3/Getopt::Long.3pm
Writing /usr/lib/perl5/5.8.8/i386-linux-thread-multi/auto/Getopt/Long/.packlist
Appending installation info to /usr/lib/perl5/5.8.8/i386-linux-thread-multi/perllocal.pod
  /usr/bin/make install  -- OK

cpan>
```

If this fails, first guess is that your system clock is out of whack.  Try `ntpdate 0.pool.ntp.org` to re-sync it. 


## References
 - [CPAN - query, download and build perl modules from CPAN sites](http://theoryx5.uwinnipeg.ca/CPAN/perl/lib/CPAN.html)