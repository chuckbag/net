# Script Arguments

Example of the code, and how to use the variables.  

```
#!/usr/bin/env python
import argparse



# - - - - - - - - - - - - - - -
# Argument Section:
parser = argparse.ArgumentParser(
    description='Input a cisco config file, and this script will parse it out, listing all the interfaces, their IPs, and descriptions',
    epilog="extra notes on how this works"
)
parser.add_argument("-d", "--debug", type=int, choices=[1, 2, 3], help="enable debug level")
parser.add_argument('configFile', help="location of switch config file to be analyzed")
parser.add_argument("-o", "--output", help="location to store output data")
args=parser.parse_args()

workfile = args.configFile
print("configFile = ", workfile)

print("output file = ", args.output)

if args.debug >= 3:
    print("debug level 3")


if args.debug >= 2:
    print( "debug level 2")
```

output would work like this: 
```
$ ./test.py -h
usage: test.py [-h] [-d {1,2,3}] [-o OUTPUT] configFile

Input a cisco config file, and this script will parse it out, listing all the
interfaces, their IPs, and descriptions

positional arguments:
  configFile            location of switch config file to be analyzed

optional arguments:
  -h, --help            show this help message and exit
  -d {1,2,3}, --debug {1,2,3}
                        enable debug level
  -o OUTPUT, --output OUTPUT
                        location to store output data
```

extra notes on how this works
```
$ ./test.py -d 2 input_file
('configFile = ', 'input_file')
('output file = ', None)
debug level 2
```
