# Function

## Define and use a function
This even includes color codes for the output!  (just to be fancy).  
```bash
RED='\033[0;31m'
GRN='\033[0;92m'
NC='\033[0m' # No Color

function HelloWorld
    printf ${GRN}Hello World!  ${NC}\n\n"
}

echo " running function: "
HelloWorld
```

