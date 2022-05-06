---
title: OpenSSL
taxonomy:
    category: docs
---

## [Symmetric encryption](https://en.wikipedia.org/wiki/Symmetric-key_algorithm)

We can use `enc` command. For more information try `man openssl-enc`. We are going to use [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) to increase security. For a simple explanation of this technique refer to this [page](https://simplicable.com/new/key-derivation-function). For generating password we are going to use `-aes-256-cbc` For more information refer to [AES](https://simplicable.com/new/key-derivation-function) and [Cipher block chaining (CBC)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher_block_chaining_(CBC)):

```
openssl enc --pbkdf2 -aes-256-cbc -in plaintext.txt -out secret.txt
```
You can try `openssl enc --list` to see all available symmetric algorithms. For decrypting we must use `-d` flag:

```
openssl enc --pbkdf2 -aes-256-cbc -d -in secret.txt -out plain.txt
```

## [Asymmetric encryption](https://en.wikipedia.org/wiki/Public-key_cryptography)

### Generating a private key
Ware going to use [RSA (Rivest–Shamir–Adleman)](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) algorithm. According to openssl documentation (try `man openssl`), `genrsa` is superseded by `genpkey`. So using the latter is recommended. For more information try `man openssl-genpkey`

```
openssl genpkey -algorithm RSA -out private.pem
```

The output is in [Privacy-Enhanced Mail (PEM)](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) format. Basically it utilizes [base64](https://en.wikipedia.org/wiki/Base64) to convert the binary format [Distinguished Encoding Rules (DER)](https://en.wikipedia.org/wiki/X.690#DER_encoding) into a text one that is easily transferable in text-based protocols like HTTP. The PEM format has the following structure:

```
-----BEGIN label-----
data
-----END label-----
```
In our case `label` is `PRIVATE KEY`:

```
-----BEGIN PRIVATE KEY-----
data
-----END PRIVATE KEY-----
```

####  The traditional method: `genrsa`

If we use `genrsa` command to generate a private key. It's in SSLeay format. Openssl documentation (try `man openssl-rsa`) recommend to use [PKCS#8](https://en.wikipedia.org/wiki/PKCS). Note that in SSLeay the label is `RSA PRIVATE KEY`, instead of `PRIVATE KEY`:

```
-----BEGIN RSA PRIVATE KEY-----
data
-----END RSA PRIVATE KEY-----
```

We're going to generate a private key in SSLeay format:
```
openssl genrsa -out ssleay.pem 2048
```
Then we convert it into PKCS#8 using the following command:

```
openssl pkcs8 -in ssleay.pem -topk8 -nocrypt -out private.pem
```

Now our private key should have the following structure (Note that label doesn't have `RSA` anymore):

```
-----BEGIN PRIVATE KEY-----
data
-----END PRIVATE KEY-----
```

### Generating a public key

We use `rsa` command to generate a public key. For more information try `man openssl-rsa`. 

```
openssl rsa -in private.pem -out public.pem -pubout
```

The public key is in PEM format. Now we can use the following command to encrypt and decrypt a file:

```
echo "hello world" > ./plain.txt
openssl rsautl -encrypt -inkey public.pem -pubin -in plain.txt -out rsa-encrypted.txt
openssl rsautl -decrypt -inkey private.pem -in rsa-encrypted.txt -out rsa-decrypted.txt
cat rsa-decrypted.txt
```

## Self-signed Certificates

If you want to have a https web server in your local network. First you need to create the certificate and then manually add it to your web browser to avoid warnings.

### Self-signed Certificate

Create a config file as below. According to OpenWrt [wiki](https://openwrt.org/docs/guide-user/luci/getting_rid_of_luci_https_certificate_warnings), browsers usually only look at one key part of a self-signed certificate that is CN (common name). However, starting with Chrome 58, SAN (subject alt name of DNS name) is also checked. So it is important in the following config `CN` matches one of `DNS` fields with a valid local DNS. Also `IP` fields should have the correct local IP address.

#### Single Domain Config

Let's assume we want to create a certificate for `foo.lan`, then `IP` should have the result IP address of the following `ping` command:

```
$ ping foo.lan
PING foo.lan (192.168.1.1)...
...
```

Using above information, we have:

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
DNS                 = foo.lan
IP                  = 192.168.1.1
```

#### Subdomains Config using Wildcards

Let's assume `192.168.1.1` hosts our web server. We want to have a certificate for `foo.lan`, `sub1.foo.lan` and `sub2.foo.lan` and all other future subdomains (e.g. `bar.foo.lan`). Let's assume from `192.168.1.2` we want to access our web server. The following `ping` command should be resolved to `192.168.1.1` from `192.168.1.2`:

```
$ ping foo.lan
PING foo.lan (192.168.1.1)...
...

$ ping sub1.foo.lan
PING foo.lan (192.168.1.1)...
...

$ ping sub2.foo.lan
PING foo.lan (192.168.1.1)...
...
```
If they don't, you need to add domain and its subdomain to `/etc/hosts` of `192.168.1.2` or use better alternatives. Now we are using the following config in `192.168.1.1`:

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
L                   = Montreal
O                   = FreeDove
OU                  = FreeDove
CN                  = *.foo.lan

[v3_req]
keyUsage            = nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage    = serverAuth
subjectAltName      = @alt_names

[alt_names]
DNS.1               = foo.lan
IP                  = 192.168.1.1
DNS.2               = *.foo.lan
```

`man x509v3_config` describes the reason for `DNS.1` and `DNS.2`:


> Due to the behaviour of the OpenSSL conf library the same field name can only occur once in a section. This means that:

```
        subjectAltName=@alt_section

        [alt_section]

        email=steve@here
        email=steve@there
```

> will only recognize the last value. This can be worked around by using the form:

```
        [alt_section]

        email.1=steve@here
        email.2=steve@there
```

#### Generate Certificate and Private Key

You can generate a certificate and a private key by:

```
$ sudo openssl req -x509 -nodes -days 730 -newkey rsa:4096 -keyout /etc/ssl/private/mycert.key -out /etc/ssl/certs/mycert.crt -config /etc/ssl/myconfig.conf
```

You can try below commands to see other options for [X.509](https://en.wikipedia.org/wiki/X.509):

```
$ man openssl-req
$ man openssl-x509
```

For more information read OpenWrt [wiki](https://openwrt.org/docs/guide-user/luci/getting_rid_of_luci_https_certificate_warnings).

#### View Certificate

You can see certificate contents by:

```
$ openssl x509 -pubkey -fingerprint -text -in /etc/ssl/certs/mycert.crt
```

The output is public key, fingerprint, signature algorithm, issuer and subject names, serial number, start and expiry dates, and so on. For more information:

```
$ man openssl-x509
```

You can see the structure of X.509 certificate in this [wiki](https://en.wikipedia.org/wiki/X.509#Structure_of_a_certificate).

### Add Self-Signed Certificate to Linux Client

We need to manually add `mycert.crt` to your favorite browser in Linux. You can use `scp` or other alternatives to copy `mycert.crt` into you client computer. Then use the following command for Chromium and Evolution:

```
$ certutil -d sql:$HOME/.pki/nssdb -A -t "CT,C,c" -n foo -i mycert.crt
```

For Firefox you need to find Firefox home directory. Usually it's in `~/.mozilla/firefox/[string].default-release` (`[string]` is a random string). Then use the following command:

```
$ certutil -d ~/.mozilla/firefox/[string].default-release -A -t "CT,C,c" -n foo -i mycert.crt
```

For viewing items in Firefox database:

```
$ certutil -d ~/.mozilla/firefox/[string].default-release -L
Certificate Nickname                                         Trust Attributes
                                                             SSL,S/MIME,JAR/XPI

foo                                                          CT,C,c
```

To delete it:

```
$ certutil -d ~/.mozilla/firefox/[string].default-release -D -n foo
```

For more information:

* [Network Security Services](https://wiki.archlinux.org/title/Network_Security_Services)
* [Adding a trusted CA certificate](https://wiki.archlinux.org/title/User:Grawity/Adding_a_trusted_CA_certificate).

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

### Useful Links

* [OpenSSL on Arch Wiki](https://wiki.archlinux.org/title/OpenSSL)
