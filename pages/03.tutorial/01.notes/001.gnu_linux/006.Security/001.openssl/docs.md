---
title: OpenSSL
taxonomy:
    category: docs
---

## Self-signed Certificates

If you want to have a https web server in your local network. First you need to create the certificate and then manually add it to your web browser to avoid warnings.

### Create a self-signed Certificate

Create a config file as below. According to OpenWrt [wiki](https://openwrt.org/docs/guide-user/luci/getting_rid_of_luci_https_certificate_warnings), browsers usually only look at one key part of a self-signed certificate that is CN (common name). However, starting with Chrome 58, SAN (subject alt name of DNS name) is also checked. So it is important in the following config `CN` matches `DNS.1` with a valid local DNS or a host name. Also `IP.1` should have the correct local IP address. Let's assume `CN` and `DNS.1` have value `foo.lan`, then `IP.1` should have the result IP address of the following `ping` command:

```
$ ping foo.lan
192.168.1.1
```

```
$ cat /etc/ssl/myconfig.conf

[req]
distinguished_name  = req_distinguished_name
x509_extensions     = v3_req
prompt              = no
string_mask         = utf8only

[req_distinguished_name]
C                   = CA
ST                  = QC
L                   = SomeCity
O                   = OpenWrt
OU                  = Home Router
CN                  = foo.lan

[v3_req]
keyUsage            = nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage    = serverAuth
subjectAltName      = @alt_names

[alt_names]
DNS.1               = foo.lan
IP.1                = 192.168.1.1
```

Then you can generate a certificate and a private key by:

```
$ sudo openssl req -x509 -nodes -days 730 -newkey rsa:2048 -keyout /etc/ssl/private/mycert.key -out /etc/ssl/certs/mycert.crt -config /etc/ssl/myconfig.conf
```

For more information read OpenWrt [wiki](https://openwrt.org/docs/guide-user/luci/getting_rid_of_luci_https_certificate_warnings).

### Add Self-Signed Certificate to Linux Client

We need to manually add `mycert.crt` to your favorite browser in Linux. You can use `scp` or other alternatives to copy `mycert.crt` into you client computer. Then use the following command for Chromium and Evolution:

```
$ certutil -d sql:$HOME/.pki/nssdb -A -t "CT,C,c" -n foo -i mycert.crt
```

For Firefox you need to find Firefox home directory. Usually it's in `~/.mozilla/firefox/[string].default-release` (`[string]` is a random string). Then use the following command:

```
$ certutil -d ~/.mozilla/firefox/[string].default-release -A -t "CT,C,c" -n foo -i mycert.crt
```

For more information read this Arch [wiki](https://wiki.archlinux.org/title/User:Grawity/Adding_a_trusted_CA_certificate).

## Checking for Expiry Date

Note that private and public keys don't have expiry date; only certificates have. Usually certificates are in X.509. An X.509 certificate contains a public key and an identity (a hostname, or an organization, or an individual), and is either signed by a certificate authority or self-signed.

### Check Certificate Expiry Date

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

### CRL Text Format
 
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
