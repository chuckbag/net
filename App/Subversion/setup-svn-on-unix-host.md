# Setup SVN on Unix Host
Setting up SVN client on your unix host:

- [Setup SVN on Unix Host](#setup-svn-on-unix-host)
  - [Install Subversion](#install-subversion)
  - [Working with Files:](#working-with-files)
    - [Adding Files](#adding-files)
    - [Checking in Changes](#checking-in-changes)
    - [Reviewing Diffs](#reviewing-diffs)
      - [Current File vs. Previous Version](#current-file-vs-previous-version)
      - [Review File's History](#review-files-history)
      - [Current File vs. Any Version](#current-file-vs-any-version)
  - [Administration](#administration)
    - [Creating a Global Repository](#creating-a-global-repository)
    - [Adding Users {anchor:addingUsers}](#adding-users-anchoraddingusers)


## Install Subversion
(assuming that your using a redhat distro)
from the command line, install svn
```bash
# yum install subversion.i386
```

## Working with Files:

### Adding Files
When you add a file, you need to first tell svn that a new file was created, before checking it in.
```bash
$ svn add *
$ svn commit -m "text describing change"
```

### Checking in Changes
When you modify a file, you then need to check it in to have those changes saved. The process is really simple. In the directory that you made the change, simply enter the command:
```bash
$ svn commit -m "what change was made"
```
And all the files modified will be checked in.

### Reviewing Diffs
With diffs, you need to understand two basic file types; binary and text. With text files, svn (and the unix diff command) can compare two files and tell you exactly what characters have changed. (For example, an extra carriage return was added, and this word was changed.) With binary files, all svn can do is tell you that a change was made, and what the comments were when checking in the new file.
The following reviews when and why all the previous versions of the file were created, and how to compare any two versions of the file.

#### Current File vs. Previous Version
You can compare the current checked in file with the previous checked in file, or if you modify a file, you can compare it with the current checked in version.
To compare a modified file with its checked in version, simply type
```bash
$ svn diff <filename> 
```

To compare the current checked in version of a file with it's previous checked in version you first need to know the previous version number. Revision numbers are global, thus a file checked in three times, could have revision numbers r3, r104, r509. Because of this, you need to run a svn log command (see below) to first see the revision history of the file to see it's last revision number. Then use the diff statement to get the diff:
```bash
$ svn diff -r <revision_number> <filename> 
```

For example:
```bash
$ svn diff -r 103 configbuilder-c3120.pl 
```

#### Review File's History
To look at the history of a file, showing the different last few check ins, use the log variable.
```bash
$ svn log <filename>
```

Note that the revision numbers are "GLOBAL" so a single files three check ins could/should have very non contiguous revision numbers.

#### Current File vs. Any Version
Being able to compare two different versions is very much the same as comparing the current file vs. the prev. (see above). the only difference is that you need to reference both revision numbers.
```bash
$ svn diff -r <revision_number>:<revision_number> <filename> 
```

For example:
```bash
$ svn diff -r 49:103 configbuilder-c3120.pl 
```

## Administration

The server is located on {{server.site.com}}, and the repositories administrative directory is located at `{{/data/svnrepository/conf}}`

### Creating a Global Repository

Technically, this is only creating another project within a current repository.

First, create some content that can be checked in to create the repository.  (This can be an empty directory, or some content.)
```bash
# mkdir /tmp/emptydir
# svn import /tmp/emptydir/ file:///data/svnrepository/project1{code}
```

Enable rights to the new repository:
```bash
# vim /data/svnrepository/conf/authz
    [/project1]
    @project1 = rw
    * =
   exit
```

### Adding Users {anchor:addingUsers}

Edit the file `*/data/svnrepository/conf/authz*` and append the new user to the end of the comma separated string.
```
[groups]
project1  = user1,user2,user3,{username}
```

Then edit the file `*/data/svnrepository/conf/passwd*` and under users add the username and password.  Enter the new username (from above), and then include the password.  (Yes it's in plaintext.  Deal.)
```
[users]
{username} = {password}
```

