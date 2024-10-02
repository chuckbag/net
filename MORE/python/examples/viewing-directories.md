# viewing directories

## Look at file in a single directory: 
code
```
import os

techaudit = '.'
directoryContents = [ f for f in os.listdir(techaudit) if os.path.isfile(os.path.join(techaudit,f))]
for f in directoryContents:
    print(f)
```
output
```
checktechaudit
tmp1.txt
tmp2.txt
```

##  look at files and folders recursively. 
code: 
```
import os

techaudit = './techaudit'

for root, dirs, files in os.walk(techaudit, topdown=False):
    for name in files:
        print("file", os.path.join(root, name))
    for name in dirs:
        print("dir", os.path.join(root, name))
```
output: 
```
file ./techaudit\ts01.prod.fra.techaudit\netstat.txt
file ./techaudit\web02.bos.techaudit\netstat.txt
file ./techaudit\wg01.prod.fra.techaudit\netstat.txt
file ./techaudit\blankfile.txt
dir ./techaudit\ts01.prod.fra.techaudit
dir ./techaudit\web02.bos.techaudit
dir ./techaudit\wg01.prod.fra.techaudit
```

