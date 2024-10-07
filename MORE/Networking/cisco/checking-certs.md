# Checking certs

Confirm what trustpoint (cert) is being presented on the external interface: 

```
LEX-ASA-FW1/act# sh run ssl
ssl server-version tlsv1-only
ssl encryption aes256-sha1 aes128-sha1 3des-sha1
ssl trust-point vpn.chuck.com-2017 outside
```

Then view that cert's details.  

```
LEX-ASA-FW1/act# sh crypto ca cert vpn.chuck.com-2017
Certificate
  Status: Available
  Certificate Serial Number: 0d5071b8432632aa2a62252e202e6d97
  Certificate Usage: General Purpose
  Public Key Type: RSA (2048 bits)
  Signature Algorithm: SHA256 with RSA Encryption
  Issuer Name:
    cn=DigiCert SHA2 High Assurance Server CA
    ou=www.digicert.com
    o=DigiCert Inc
    c=US
  Subject Name:
    cn=vpn.chuck.com
    ou=Information Technology
    o=chuck
    l=Boston
    st=Massachusetts
    c=US
  OCSP AIA:
    URL: http://ocsp.digicert.com
  CRL Distribution Points:
    [1]  http://crl3.digicert.com/sha2-ha-server-g1.crl
    [2]  http://crl4.digicert.com/sha2-ha-server-g1.crl
  Validity Date:
    start date: 20:00:00 EDT Sep 12 2017
    end   date: 08:00:00 EDT Sep 18 2018
  Associated Trustpoints: vpn.chuck.com-2017
LEX-ASA-FW1/act#
```
