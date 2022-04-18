# commented commits

When you make a commit, you can add a comment to it so that you can review the different changes that were made.

```bash
[edit]
root@mb2-fw1b01# commit comment "--clean start--"
commit complete
```

then exit out of the edit mode and run the following to see each commit version.  The highlighted one is the commented version that was just checked in
```bash
root@mb2-fw1b01> show system commit
0   2018-02-07 20:21:04 UTC by root via cli
    --clean start--
1   2018-02-07 20:19:11 UTC by root via cli
2   2018-02-07 19:41:31 UTC by root via cli
3   2018-02-07 19:40:37 UTC by root via cli
4   2018-02-07 19:15:25 UTC by root via cli
5   2018-02-07 18:51:44 UTC by root via cli
```

If your in edit mode "run" the command above: 
```bash
root@mb2-fw1b01# run show system commit
0   2018-02-07 20:21:04 UTC by root via cli
    --clean start--
1   2018-02-07 20:19:11 UTC by root via cli
2   2018-02-07 19:41:31 UTC by root via cli
3   2018-02-07 19:40:37 UTC by root via cli
4   2018-02-07 19:15:25 UTC by root via cli
5   2018-02-07 18:51:44 UTC by root via cli

[edit]
root@mb2-fw1b01#
```


## Reference: 
- [Adding a Comment to Describe the Committed Configuration](https://www.juniper.net/documentation/en_US/junos/topics/task/configuration/junos-software-committed-configuration-comments-adding.html): Juniper, Nov 2017