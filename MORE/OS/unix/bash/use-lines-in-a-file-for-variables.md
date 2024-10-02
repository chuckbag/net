# Use lines in a file for variables

print out each line of a file called file.txt
```bash
#!/bin/bash
file="file.txt"

while read line; do
     printf "$line \n"
done < $file
```


