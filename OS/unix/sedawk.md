# sed/awk

<img src="img/TunJBVJ.jpeg">

(It's not so bad...)

- [sed/awk](#sedawk)
  - [sed](#sed)
  - [awk](#awk)
    - [data from columns delimited by whitespace and with specific match](#data-from-columns-delimited-by-whitespace-and-with-specific-match)
    - [Break a string by a defined delimiter, and print out the columns](#break-a-string-by-a-defined-delimiter-and-print-out-the-columns)
    - [Sending awk output to variables, not console](#sending-awk-output-to-variables-not-console)
    - [Comparing two files](#comparing-two-files)
      - [What Matches:](#what-matches)
      - [What does NOT Match:](#what-does-not-match)
    - [Pivot Tables:](#pivot-tables)
      - [Pivot Sum](#pivot-sum)
      - [Pivot Add](#pivot-add)
  - [References](#references)


## sed

Sed is the simple `s///` search/replace method that your already used to in vi.  
On MacOS, sed is a bit of a mess trying to work with newlines, so it's easier to install gnu sed with the command "`brew install gnu-sed`" and use it.  

add the line "`qty, description, $ea, $total`" at the top of the file:  
```
gsed -i '1s/^/qty, description, $ea, $total \n/' file.txt
```

Where: 
- `-i` inserts 
- `s///` is the standard search and replace
- `1` in front of the search statement says to insert this at line 1 (the top)


## awk

### data from columns delimited by whitespace and with specific match
if the directory is such: 
```
cmercier@Balsa ~/ $ ls -la
total 112
drwxr-xr-x  11 cmercier  staff   352B Sep 24 11:21 ./
drwxr-xr-x   6 cmercier  staff   192B May 24 22:58 ../
-rw-r--r--@  1 cmercier  staff   6.0K Sep 24 11:21 .DS_Store
-rw-r--r--   1 cmercier  staff   3.4K Jun 29 11:31 CFG_DB01_install.sh
-rwxr-xr-x@  1 cmercier  staff    11K May 18 11:17 postImagePreDeployCentOS7-2.sh*
-rwxr-xr-x@  1 cmercier  staff    12K May 24 16:27 postImagePreDeployCentOS7.sh*
-rwxr-xr-x@  1 cmercier  staff   1.6K Sep 17 09:59 pushIptablesChanges.sh*
-rwxr-xr-x@  1 cmercier  staff   1.4K May 18 11:28 pushToAllProd.sh*
-rwxr-xr-x@  1 cmercier  staff   361B May 18 11:28 updateHosts.sh*
-rwxr-xr-x@  1 cmercier  staff   2.5K Sep 24 11:20 updateIptables.sh*
-rwxr-xr-x@  1 cmercier  staff   3.5K May 18 11:26 updateNfsConfigs.sh*
```

output the first and last column, but only with including "update": 
```
cmercier@Balsa ~/ $ ls -la | awk '/update/ {print $1, $9;}'
-rwxr-xr-x@ updateHosts.sh*
-rwxr-xr-x@ updateIptables.sh*
-rwxr-xr-x@ updateNfsConfigs.sh*
```

Same thing, but here not the last (9th) column, but the second to last (8th) column: 
```
cmercier@Balsa ~/ $ ls -la | awk '/update/ {print $1, $8;}'
-rwxr-xr-x@ 11:28
-rwxr-xr-x@ 11:20
-rwxr-xr-x@ 11:26
```

### Break a string by a defined delimiter, and print out the columns
View the entire hostname
```
[root@dude01 ~]# hostname
dude01.mb2.prod.chuck.com
```

separate the fqdn by ".", and output the 3rd word
```
[root@dude01 ~]# echo $HOSTNAME | awk -F. '{print $3}'
prod
```

separate the fqdn by ".", and output the 2nd word
```
[root@dude01 ~]# echo $HOSTNAME | awk -F. '{print $2}'
mb2
```

separate the fqdn by ".", and output the 1st AND 4th word:
```
[root@dude01 ~]# echo $HOSTNAME | awk -F. '{print $1} {print $4}'
dude01
chuck
```

### Sending awk output to variables, not console
if you want to use awk in a script, and want the output to be assigned to a variable use the var=$() syntax
```
[root@dude01 ~]# foo=$(echo $HOSTNAME | awk -F. '{print $1} {print $4}')
[root@dude01 ~]# echo $foo
dude01 chuck
```

### Comparing two files
We can take two files, 
```
$ cat file1.txt
GERMANY
FRANCE
UK
POLLAND
$ cat file2.txt
POLLAND
GERMANY
```

#### What Matches: 
and output only the lines that match the two files: 
```
$ awk 'FNR==NR{a[$0];next}($0 in a){print}' file1.txt file2.txt
POLLAND
GERMANY
```

Where: 

- the first file `file1.txt` is copied into array `a[]` 
- the variable `$0` is the [entire line being matched](https://java2blog.com/awk-print-1/), not just the first (`$1`) or second field (`$2`).  
- the command will go through all of `file2.txt` line by line, and see if there are any matches from the array (`file1.txt`).  Each match is outputted (thus in the order of `file2.txt`). 
- the comparison `($0 in a)` uses the entire line of `file2.txt` as the index to the array, so it will only match exact line matches
- the `{print}` command will print the entire line.  You could only print the 2nd column by modifying it to `{print $2}` 
- and see [more details and explanations of FNR and NR here](http://net.cmed.us/Home/sedawk/awk-dual-file-compare)

#### What does NOT Match: 
We can do the opposite and see what data is in file1 that is not in file2.  
```
$ awk 'FNR==NR{a[$0];next}(!($0 in a)){print}' file2.txt file1.txt
FRANCE
UK
$
```

Or what's in file2  that's not in file1  (nothing!)
```
$ awk 'FNR==NR{a[$0];next}(!($0 in a)){print}' file1.txt file2.txt
$
```


### Pivot Tables: 
There are more details in the following link on [Pivot tables with awk](http://net.cmed.us/Home/sedawk/pivot-tables-with-awk), but the summary is the following: 

We have a table with data like the following: 
```
fruit   qty  store   price
apple   23  walmart  5
apricot   235 giant    4
avocado   576 kroger   5
```

#### Pivot Sum
The awk command would be the following: 
```
awk 'BEGIN {FS=OFS=","} \
NR>1 \
{count[$1]++} \
END {for (word in count) {print word, count[word]}}' \
$1 | sort -n
```

And the output would look like the following: 
```
$ ./pivot-file2.sh file2.csv
apple,4
apricot,3
avocado,4
banana,1
blackberry,6
blueberry,1
```

#### Pivot Add
The awk command would be the following: 
```
awk 'BEGIN {FS=OFS=","} \
NR>1 \
{fruit[$1]+=$2} \
END {for (x in fruit) {print x,fruit[x]}}' \
$1 | sort -n
```

And the output would look like the following: 
```
$ ./pivot-file1.sh file1.csv
apple,185
apricot,43
avocado,23
banana,290
blackberry,702
blueberry,588
```


## References
- [8 Powerful Awk Built-in Variables – FS, OFS, RS, ORS, NR, NF, FILENAME, FNR](https://www.thegeekstuff.com/2010/01/8-powerful-awk-built-in-variables-fs-ofs-rs-ors-nr-nf-filename-fnr/): the geek stuff, Jan 2010
- [Saving awk output to variable](https://stackoverflow.com/questions/18648345/saving-awk-output-to-variable): stackoverflow, Sep 2011
- [awk](https://en.wikipedia.org/wiki/AWK): wikipedia
- [Command Line Tutorials – Sed & Awk: Jessica Dillon](https://quickleft.com/blog/command-line-tutorials-sed-awk/), Aug, 2012
