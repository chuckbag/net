# confirmed commits

## Overview: 
if your afraid that your change might lock you out of your system, then use this to make sure that all will go well.  

## Confirmed Commits: 
Make changes that will automatically be removed if not accepted.

### Install new config change with option to auto backout: 
rather then simply entering the commit command to implement your change, enter commit confirmed and then after an amount of time, enter again commit.  If you don't, then the change will automatically be backed out.  The default time before a back-out is 5 min.  
```
commit confirmed
! the above will make the change, but will undo in 5 min (def) unless you enter the following: 
commit
```

### Same as above but changing the default time:
You can modify the commits "back out time" by entering the number of minutes after the confirmed statement.  The range is 1-535 minutes (~9hours).
In this example, the commit will be backed out after one minute. 
```
commit confirmed 1
! do your tests, then enter the following in less then one minute
commit
```


## Rollback
As always, if you need to undo the last change right away, you can use the rollback command: 
```
rollback 1 
```

This reverts to the config just before you entered the commit confirmed (or commit) command.  Changing the number will bring previous versions instead of the most recent.  

## References: 
- [juniper cli user guide "commit"](https://www.juniper.net/techpubs/en_US/junos/topics/reference/command-summary/commit.html): Sep 23, 2013
- [srx getting started - commit configuration](http://kb.juniper.net/InfoCenter/index?page=content&id=KB15721): Juniper kb15721, Dec 12, 2012
- [Activating a Junos Configuration but Requiring Confirmation](http://www.juniper.net/techpubs/en_US/junos10.4/topics/task/configuration/junos-cli-configuration-activating-after-confirming.html): Oct 6, 2010
- [rollback](http://www.juniper.net/techpubs/en_US/junos14.2/topics/reference/command-summary/rollback.html): Junos OS 14.2, Oct 22, 2014
