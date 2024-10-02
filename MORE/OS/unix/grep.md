# grep

## Recursive and Don't Match: 

Serach through all files in this dir, and look for lines that DO NOT INCLUDE the word chuck.
```
grep -v chuck *
```

Search through all the files in this dir and the ones below (recursive) and find all lines with the word chuck
```
grep -r chuck *
```


## Include N number of lines before/after match

Search though a file for a specific word (chuck), then show the 4 lines after it: 
```
grep -A4 chuck
```

Search though a file for a specific word, then show the 2 lines before it: 
```
grep -B2 chuck
```

Search though a file for a specific word, then show the 3 lines before and after it: 
```
grep -C3 chuck
```

## This or That
Match on two different things: (display all lines that have "this" and also display all lines that have "that")
```
grep -E '(this|that)'
```

