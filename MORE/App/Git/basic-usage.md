# Basic Git Usage

- [Basic Git Usage](#basic-git-usage)
  - [Basic Check in](#basic-check-in)
    - [View Status](#view-status)
    - [Ignore a file](#ignore-a-file)
    - [Add Files (git add)](#add-files-git-add)
    - [Commit the changes (git commit)](#commit-the-changes-git-commit)
  - [Diff Files](#diff-files)
    - [Diff a file:](#diff-a-file)
    - [Diff with external tool](#diff-with-external-tool)
  - [Github Pull/Push](#github-pullpush)
    - [Grab latest from github (git pull)](#grab-latest-from-github-git-pull)
    - [Push your updates to github (git push)](#push-your-updates-to-github-git-push)
    - [Pulling back git files that were deleted locally](#pulling-back-git-files-that-were-deleted-locally)

## Basic Check in

### View Status
Go into a git repo, and see what files are modified
```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

 modified:   .DS_Store
 modified:   configs/srx100.conf

Untracked files:
  (use "git add <file>..." to include in what will be committed)

 deployments/switch-dmz-to-dhcp.txt
 other/

no changes added to commit (use "git add" and/or "git commit -a")
$
```

### Ignore a file
The ".DS_Store" file should be ignored because of the .gitignore file, but for this example, we can force it to be ignored
```bash
$ git rm --cached .DS_Store
rm '.DS_Store'
$
```

Then to confirm that the file will be deleted from the git repo.  (The file will not be removed in the directory, it just will be removed from the git repo.)
```bash
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

 deleted:    .DS_Store

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

 modified:   configs/srx100.conf

Untracked files:
  (use "git add <file>..." to include in what will be committed)

 deployments/switch-dmz-to-dhcp.txt
 other/
$
```
### Add Files (git add)
In this example, we want to add the files in the "active_projects" directory (one is a rename, the other is an add).  We add files to what we want to commit with the add command.  
```bash
cmercier@Balsa ~/techops $ git add shared/cmercier/active_projects
cmercier@Balsa ~/techops $
```

Note, that if we added too much for this checkin (but wanted to check it in with a different group of files), we could remove it with the reset command

Confirm what files are to be committed
```bash
cmercier@Balsa ~/techops $ git status
On branch vxy-charlie
Your branch is ahead of 'origin/vxy-charlie' by 4 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

 renamed:    shared/cmercier/active_projects/20180404-FHM-ready-for-csv3-deployment -> shared/cmercier/active_projects/20180404-FHM-MB2-ready-for-csv3-deployment
 new file:   shared/cmercier/active_projects/20180525-Israel InfoSec Reg

Untracked files:
  (use "git add <file>..." to include in what will be committed)

 services/software/hp/
 shared/cmercier/other/mindmap_store/

cmercier@Balsa ~/techops $
```

### Commit the changes (git commit)
Take the changes, and check them into the repository, and track the status of the change.  
```bash
cmercier@Balsa ~/techops $ git commit -m "edit - update active projects list"
[vxy-charlie 9c8465e] edit - update active projects list
 2 files changed, 0 insertions(+), 0 deletions(-)
 rename shared/cmercier/active_projects/{20180404-FHM-ready-for-csv3-deployment => 20180404-FHM-MB2-ready-for-csv3-deployment} (100%)
 create mode 100644 shared/cmercier/active_projects/20180525-Israel InfoSec Reg
cmercier@Balsa ~/techops $
```

## Diff Files

### Diff a file: 
To see what has changed, you can use the standard unix diff-like tool to see what change has happened between the previous file that was checked in, and what the newest version is.  
```bash
$ git diff --cached configs/srx100.conf
diff --git a/configs/srx100.conf b/configs/srx100.conf
index d7d0f31..0f00111 100644
--- a/configs/srx100.conf
+++ b/configs/srx100.conf
@@ -1,4 +1,4 @@
-## Last commit: 2017-11-11 10:23:06 UTC by chuck
+## Last commit: 2018-08-14 22:14:02 UTC by chuck
 version 11.2R4.3;
 system {
     host-name srx100-a;
@@ -88,7 +88,7 @@ interfaces {
             description "dmz network";
             vlan-id 10;
             family inet {
-                address 198.18.0.254/24;
+                dhcp;
             }
         }
         unit 11 {
@@ -355,6 +355,7 @@ security {
                             ping;
                             ike;
                             https;
+                            dhcp;
                         }
                     }
                 }
$
```

### Diff with external tool
you can view the changes to the file from the previous checking using either git diff or a by using an external diff tool.  When running the command, git offers up the ability to diff each of the files that have been modified, and you can select which file to view.  
```bash
cmercier@Balsa ~/techops $ git difftool

Viewing (1/4): 'documentation/deployment/2017/11/20171102-MB2 Initial Buildout/planned flows.graffle/data.plist'
Launch 'p4merge' [Y/n]? n

Viewing (2/4): 'documentation/deployment/2018/05/20180521-forman install/debug/Forman troubleshooting.docx'
Launch 'p4merge' [Y/n]? n

Viewing (3/4): 'services/configs/fhm/fw/fw1b02.conf'
Launch 'p4merge' [Y/n]? y

Viewing (4/4): 'shared/cmercier/active_projects/20180404-FHM-ready-for-csv3-deployment'
Launch 'p4merge' [Y/n]? n
cmercier@Balsa ~/techops $
```

I like to use perforce's merge tool for my diff'ing

## Github Pull/Push
Git by itself is a very decentralized system for maintaining code or files.  If you want to put some structure into the process, and be able to view files with an HTML frontend, then github is a nice addition.  

### Grab latest from github (git pull)
To confirm that we have the most update files from github, do a pull request
```bash
cmercier@Balsa ~/techops $ git pull
remote: Counting objects: 1, done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (1/1), done.
From github.com:variantyx/techops
   f625300..bdd416c  master     -> origin/master
Already up-to-date.
cmercier@Balsa ~/techops $
```

### Push your updates to github (git push)
To take your changes and push them up to github, do a push request
```bash
cmercier@Balsa ~/techops $ git push
Counting objects: 71, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (68/68), done.
Writing objects: 100% (71/71), 55.11 MiB | 4.74 MiB/s, done.
Total 71 (delta 28), reused 0 (delta 0)
remote: Resolving deltas: 100% (28/28), completed with 16 local objects.
To github.com:variantyx/techops.git
   44ef94d..9c8465e  vxy-charlie -> vxy-charlie
cmercier@Balsa ~/techops $
```

### Pulling back git files that were deleted locally
If you have a deleted local file that you want to have replaced from the git repo, use the checkout argument.  

Confirm what file is missing
```bash
cmercier@Balsa ~/techops $ git status
On branch vxy-charlie
Your branch is up-to-date with 'origin/vxy-charlie'.

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

 deleted:    services/apps/dns/etc-fhm/named/named.conf.local
 deleted:    services/apps/dns/etc-mb2/named/named.conf.local
 deleted:    services/apps/dns/var/named/mb2/34.36.10.in-addr.arpa
 deleted:    services/apps/dns/var/named/mb2/36.36.10.in-addr.arpa

no changes added to commit (use "git add" and/or "git commit -a")
cmercier@Balsa ~/techops $
```

replace the deleted files with the versions from the git repo: 
```bash
cmercier@Balsa ~/techops $ git checkout -- services/apps/dns/
cmercier@Balsa ~/techops $
```

And confirm: 
```bash
cmercier@Balsa ~/techops $ git status
On branch vxy-charlie
Your branch is ahead of 'origin/vxy-charlie' by 7 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
cmercier@Balsa ~/techops $
```




