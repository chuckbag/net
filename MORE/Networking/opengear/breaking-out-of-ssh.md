# Breaking out of ssh

## Break out
When you log into a terminal that is connected to a console port, you get the prompt from the device as if you were directly connected to it over rs232.
```
login:
Amnesiac (ttyu0) 
```

if you type the following three keys one at a time (enter, tilde, period), you can break out of the ssh session: 
```
[Enter], [~], [.]
```

It looks like the following, where the green is the console session from a juniper router, and the blue is the terminal from a unix host that initiated the ssh session to the opengear console server.  
```
login:
Amnesiac (ttyu0)
login: Connection to 192.168.0.29 closed.
$
```

## Other Controls
Other then terminating the ssh session, there are other commands you can send over the console session.  By pressing [Enter] and [~], you are prompted with the following help page to remind you of your options.  
```
login: ~?

Supported escape sequences:
 ~.   - terminate connection (and any multiplexed sessions)
 ~B   - send a BREAK to the remote system
 ~C   - open a command line
 ~R   - request rekey
 ~V/v - decrease/increase verbosity (LogLevel)
 ~^Z  - suspend ssh
 ~#   - list forwarded connections
 ~&   - background ssh (when waiting for connections to terminate)
 ~?   - this message
 ~~   - send the escape character by typing it twice
(Note that escapes are only recognized immediately after newline.)
```

So if you wanted to send a break to a starting cisco router, you would enter the three keys one after the other `[Enter]`, `[~]`, `[B]`

## Reference
- [What escape sequences and pmshell menu commands are available inside a console session: Opengear](https://opengear.zendesk.com/hc/en-us/articles/216373723-What-escape-sequences-and-pmshell-menu-commands-are-available-inside-a-console-session-), Robert Wildie, 2017