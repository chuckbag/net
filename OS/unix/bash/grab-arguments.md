# Grab Arguments


## Problem: 
In a bash script have a smart method for going through all the arguments and doing stuff with them.  You need to be able to have two different kinds of arguments, one is a flag or something defined, and the other is some input to do something with.  For example you might have two arguments "`-p 1.1.1.1`" which would tell the script to ping 1.1.1.1, but then you might also have an additional argument "`-d`" which tells the script to spit out debug output as well as everything else.  

## Example 1
In this example, we will look for "options" and "arguments" following a script.  Anything that starts with a "-" in this script is what I'm calling an option. Anything without the dash I'm calling an argument.  

Like this: 
```bash
./myScript -option argument -option -option argument argument
```

The script would run like the following: 
```bash
$ ./bash-arguments.sh -a b --opt1 -b x y z
option 1
argument b
bad option --opt1
option 2
argument x
argument y
argument z
```

The script is the following.  It's looking for either "-a" or "-b" for "options".  Anything else with a "-" in front is considered a "bad option".  Anything without a "-" is considered an argument.  
```bash
#!/bin/bash
#
#
while test $# -gt 0

do
    case "$1" in
        -a) echo "option 1"
            ;;
        -b) echo "option 2"
            ;;
        -*) echo "bad option $1"
            ;;
        *) echo "argument $1"
            ;;
    esac
    shift
done
```

## Example 2
Take the same and do some stuff with it.  Here if we only want to do stuff if we provide "`-a {andsomestuff}`" and also if we do a "`-b`".  Everything else can be ignored.  

Code: 
```bash
#!/bin/bash
#
#
echo "provide the following: "
echo "either -a and a value"
echo "or -b"

while test $# -gt 0

do
    case "$1" in
        -a) echo ">option a"
        VAL="a"
        echo "    VAL = $VAL"
            ;;
        -b) echo ">option b"
        echo "    -> do something related to b <- "
        VAL="b"
            ;;
        -*) echo ">bad option $1"
        echo "    maybe do something related to getting a bad option"
        VAL=""
            ;;
        *) echo ">argument $1"
        echo "    VAL = $VAL"
        if [[ $VAL == "a" ]]
        then
            echo "    -> run function a, and pass it argument [$1] <-"
        fi
        VAL=""
            ;;
    esac
    shift
done
```

Output: 
```bash
$ ./bash-arguments.sh -a 1111 -b ggg
provide the following:
either -a and a value
or -b
>option a
    VAL = a
>argument 1111
    VAL = a
    -> run function a, and pass it argument [1111] <-
>option b
    -> do something related to b <-
>argument ggg
    VAL = b
$
```

## References: 
- [Check if any of the parameters to a bash script match a string](https://superuser.com/questions/186272/check-if-any-of-the-parameters-to-a-bash-script-match-a-string): Sep 2010, superuser.com
- [How To Pass and Parse Linux Bash Script Arguments and Parameters](https://www.poftut.com/how-to-pass-and-parse-linux-bash-script-arguments-and-parameters/): Sep, 2017 by İsmail Baydan
