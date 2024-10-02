# dig

Detailed answer
```bash
$ dig www.gwu.edu

; <<>> DiG 9.7.0-P1 <<>> www.gwu.edu
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 21046
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;www.gwu.edu.                   IN      A

;; ANSWER SECTION:
www.gwu.edu.            60      IN      A       128.164.131.179

;; Query time: 252 msec
;; SERVER: 10.241.36.15#53(10.241.36.15)
;; WHEN: Mon Jun  6 10:22:15 2011
;; MSG SIZE  rcvd: 45
```

brief answer
```bash
$ dig www.gwu.edu +noall +answer

; <<>> DiG 9.7.0-P1 <<>> www.gwu.edu +noall +answer
;; global options: +cmd
www.gwu.edu.            35      IN      A       161.253.149.179
```

short answer:
```bash
$ dig www.gwu.edu +short
128.164.131.179
```

reverse:
```bash
$ dig -x 204.152.184.167 +short
mx.isc.org.
```

different nameserver
```bash
$ dig @ns1.google.com www.google.com
```

## Ref:
- [Domain Information Groper (dig)](http://en.wikipedia.org/wiki/Domain_Information_Groper), Wikipedia.
- [nslookup and dig](http://docstore.mik.ua/orelly/networking_2ndEd/dns/ch12_01.htm), Orelly's Networking (2nd Ed)
- [dig man](http://linux.die.net/man/1/dig)  Linux man page
- [DiG HOWTO](http://www.madboa.com/geek/dig/) by Paul Heinlein
