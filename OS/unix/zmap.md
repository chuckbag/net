# zmap

## Simple ping scan: 
```bash
zmap --probe-module=icmp_echoscan -B 10M -o results.csv 10.1.1.0/24
```
where
- `-B 10M` states to use no more than 10MB bandwidth while running the test (IMPORTANT!!!)
- `--probe-module=icmp_echoscan` Tells zmap to scan using the icmp echoscan module 
- `-o` results.csv lists the file to save the results/output to.  

## Web Port Scan: 
```bash
zmap -B 10M -p 80  -o results.csv 10.1.1.0/24
```
where 
- `-B 10M` states to use no more than 10MB bandwidth while running the test (IMPORTANT!!!)
- `-p 80` lists the ports to scan (tcp & udp)
- `-o results.csv` lists the file to save the results/output to.  

## Scan Multiple Ports: 
```bash
zmap -B 10M -p0-65535  -o results.csv 10.1.1.0/24
```
where 
- `-B 10M` states to use no more than 10MB bandwidth while running the test (IMPORTANT!!!)
- `-p0-65535` scans all of the ports (tcp & udp)
- `-o results.csv` lists the file to save the results/output to. 

## Scan Multiple IPs
ICMP scan multiple /24 networks
```bash
zmap --probe-module=icmp_echoscan -B 10M -o results.csv 10.1.1.0/24 10.10.0.0/24
```
where
- `-B 10M` states to use no more than 10MB bandwidth while running the test (IMPORTANT!!!)
- `--probe-module=icmp_echoscan` Tells zmap to scan using the icmp echoscan module 
- `-o results.csv` lists the file to save the results/output to.  

## References: 
- [zmap](https://zmap.io/)  
- [Here's what you find when you scan the entire Internet in an hour](http://www.washingtonpost.com/blogs/the-switch/wp/2013/08/18/heres-what-you-find-when-you-scan-the-entire-internet-in-an-hour/?hpid=z10). Washington Post  Timothy Lee, Aug 18, 2013