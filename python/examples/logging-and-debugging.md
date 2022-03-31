# Logging and Debugging

## Log to screen: 

### Print output at debug level
set the `level=logging` to `DEBUG ` 

Code: 
```
import logging

# get logging setup:
logging.basicConfig(level=logging.DEBUG, format='%(asctime)s - %(levelname)s - %(message)s')

# print a basic message in DEBUG 
logging.debug('> This is a log message.')

# print a message with a variable in INFO + DEBUG
y = "my variable"
logging.info('> text & {}'.format(y))
```
output: 
```
2015-11-02 17:50:49,349 - DEBUG - > This is a log message.
2015-11-02 17:50:49,349 - INFO - > text & my variable

Process finished with exit code 0
```

### print output at info level: 
Just set the `level=logging` to `INFO` 

code
```
import logging

# get logging setup:
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

# print a basic message in DEBUG
logging.debug('> This is a log message.')

# print a message with a variable in INFO + DEBUG
y = "my variable"
logging.info('> text & {}'.format(y))
```
output
```
2015-11-02 17:52:58,798 - INFO - > text & my variable

Process finished with exit code 0
```


## References: 
- [Stop Using "print" for Debugging: A 5 Minute Quickstart Guide to Python’s logging Module: Al Sweigart, April, 2012](http://inventwithpython.com/blog/2012/04/06/stop-using-print-for-debugging-a-5-minute-quickstart-guide-to-pythons-logging-module/)
- [Logging facility for Python: v2.7.10](https://docs.python.org/2/library/logging.html)
- [Python Logging HOWTO: Vinay Sajip. v2.7.10](https://docs.python.org/2/howto/logging.html)

