# system calls with arrays

## Overview

We can use the system() command to safely run system calls.  normally this would be done like this: 
system( "ls", "-lf" )
but if you want to have the variables within array and still use the system procedure, you need to do the following: 

## The Code
```
#!/usr/bin/ruby

cmds = Array.new    # define new array
cmds.push("ls")     # put in first value
cmds.push("-lF")    # put in second value

puts "--- the variables in the command are:"
puts cmds
puts "--- and the results are: "
puts " "

system( *cmds )
```

## The Results: 
```
--- the variables in the command are:
ls
-lF
--- and the results are:

total 4
-rwxrwxr-x 1 chuck chuck 302 Oct 16 15:56 test.rb*
```


