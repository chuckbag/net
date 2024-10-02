# Usage Statement
Add this so you know what the script does and how to use it
```bash
# ---------------------------------------------------
# 1. usage for this script

# 1.1 define usage: 
programname=$0
function usage {
    echo "usage: $programname [-h] "
    echo "This is what we are doing here"
    echo ""
    echo "  -h            display help"
    echo "  -i {IP}       single IP address to use"
    echo "  -f {infile}   file with list of IPs to use"
    exit 1
}

# 1.2 if help, display usage
if [[ ( $@ == "--help") ||  $@ == "-h" ]] 
then         
    usage
    exit 0
fi 
# 1.3 if less than two arguments supplied, display usage 
if [  $# -le 1 ] 
then 
    usage
    exit 1
fi 
```
