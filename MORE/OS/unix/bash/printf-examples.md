# Printf - Examples

## Problem
Want to be able to do more then simply echoing out data to the console.  For example, maybe echo out text that is color coded.  

## Example 1
You can define colors in the header of your script, and then using the `printf` command, and include those colors.  

Note that the [bash color codes are noted here](shell-color-coding.md).

### Code: 

``` bash
#!/bin/bash
#

# ---------------------------------------------------
# 0. Global Variables: 
RED='\033[0;31m'
GRN='\033[0;92m'
NC='\033[0m' # No Color

# ---------------------------------------------------
printf "${RED} === This is red text with a carriage return after \n"
printf "${GRN} === This is green text ==="

variableWord="some phrase"

printf "${NC} And this is text with the variable [ $variableWord ] an two carriage returns after \n\n"
```

### Output

<img src="../img/2022-11-04_16-02-52.png" alt="output that shows the output text color">
