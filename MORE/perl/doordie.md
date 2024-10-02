# doOrDie


```
#!/usr/bin/perl
#
# open the file "$file".  If it can not, continue but
# send out a warning.  (rather then the usual die.)
#
# note that this is just a snippit, not a fully running code

        open(FH, $file) or do {
                warn "foo!";
                next;
        };
```

