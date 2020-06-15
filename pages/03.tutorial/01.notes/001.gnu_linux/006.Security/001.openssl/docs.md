---
title: OpenSSL
taxonomy:
    category: docs
---

# Checking for Expiry Date

Note that private and public keys don't have expiry date; only certificates have. Usually certificates are in X.509. An X.509 certificate contains a public key and an identity (a hostname, or an organization, or an individual), and is either signed by a certificate authority or self-signed.

## Check Certificate Expiry Date

You can open certificate  file (the extension can be `crt`, `pem`, ...)  with a text editor or running the following command to see its expiry date:

```
openssl x509 -enddate -noout -in file.crt
```

For printing both start and  end date:

```
openssl x509  -startdate -enddate -noout -in file.crt
```

or you can use the following command:

```
openssl x509 -dates -noout -in file.crt
```

For more information run `man openssl-x509` or `openssl x509 -help`.

## CRL Text Format
 
If you want to see a CRL file (certificate revocation list) `last update` and `next update` (expiry date), you can run:

```
openssl crl -lastupdate -nextupdate -noout -in crl.key

```

If you want to see its text version:

```
openssl crl -text -in crl.pem
```

If you don't want to see the actual CRL, use the following command:

```
openssl crl -text -noout -in crl.pem
```

For more information run `man openssl-crl` or `openssl crl -help`.
