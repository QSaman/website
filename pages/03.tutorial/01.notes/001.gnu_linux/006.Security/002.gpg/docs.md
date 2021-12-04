---
title: GPG
taxonomy:
    category: docs
---

## GPG vs. PGP vs. OpenPGP

[GNU Privacy Guard (GPG)](https://en.wikipedia.org/wiki/GNU_Privacy_Guard) is available out of the box in most GNU/Linux Distros. Both GPG and the proprietary [Pretty Good Privacy](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) implement OpenPGP standard.

## Useful sites

* [How To Geek](https://www.howtogeek.com/427982/how-to-encrypt-and-decrypt-files-with-gpg-on-linux/)

## Listing Current Fingerprints

To see the current [public key fingerprints](https://en.wikipedia.org/wiki/Public_key_fingerprint) run the following command:

```
gpg --fingerprint
```

## Signing and Verifying a document

There are three types of signing in GPG. For more information visit [GNU PG](https://www.gnupg.org/gph/en/manual/x135.html) and this [post](https://superuser.com/questions/1579649/how-to-fix-warning-not-a-detached-signature).

### `--sign`

In this case the document is compressed before signed, and the output is in binary format. By default the extension is `.gpg`.

```
$ gpg --sign foo.jpg
$ ls
foo.jpg.gpg
```

You can verify it using the following command:

```
$ gpg --verify foo.jpg.gpg
```

You can verify and extract the file using the following command:

```
$ gpg --output my_foo.jpg --decrypt foo.jpg.gpg
$ ls
my_foo.jpg
```
Note that by default `gpg` write the content of file into `stdout`, so we used `--output`.

### `--clear-sign`

If the document that you want to sign is a text file (e.g. an email message), you can used `--clear-sign`. By default the extension is `asc` which is an ASCII armor file.

```
$ gpg --clear-sign foo.txt
$ ls
foo.txt.asc
```

It doesn't compress the data and generate a document like:

```
$ cat foo.txt
Line1: message
Line2: message
```

```
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

Line1: message
Line2: message
-----BEGIN PGP SIGNATURE-----

My message signature in base64
-----END PGP SIGNATURE-----
```

It's not a good idea to use `clear-sign` for binary files. You have difficulty for retrieving the original file.

### `--detach-sign`

Instead of writing both the document and its signature into a file, it writes the signature in a separate file

```
$ gpg --detach-sign foo.jpg
$ ls
foo.jpg
foo.jpg.sig
```

If you want to generate an ASCII armor signature file:

```
$ gpg --armor --detach-sign foo.jpg
$ ls
foo.jpg
foo.jpg.asc
```

For verifying you need to have both original document and its signature:

```
$ gpg --verify foo.jpg.asc
$ gpg --verify foo.jpg.asc foo.jpg
```

# Verifying Integrity of a File

First you need to import the public key to verify the file. Usually you are provided with the fingerprint of the public key and maybe the public key itself. There are two options to import it.

## Import Public Key Directly

First you need to download the public key from the website that has `asc` extension. It's a public key in Base64 format which is called `ASCII armor` file in OpenGPG [Standard](https://en.wikipedia.org/wiki/Base64#OpenPGP).

You can check fingerprint of the public key without importing it by running this command:

```
gpg --keyid-format long --import --import-options show-only --with-fingerprint public_key.asc
```

Then check the output and make sure fingerprint is the one that you expect. If you see a message like `signatures not checked due to missing keys` then you can use [OpenGPG Web of Trust](https://en.wikipedia.org/wiki/Web_of_trust) to remove it. For example you can import the public key of a well-known Debian developer like Jan Dittberner from Debian keyring [website](https://keyring.debian.org/) using the following command:

```
gpg --recv-keys B2FF1D95CE8F7A22DF4CF09BA73E0055558FB8DD
```
You can check who sign a specific public key using this command:

```
gpg --list-sigs [fingerprint]
```

For more information visit this [website](https://www.debian.org/events/keysigning) and also [this](https://infra.apache.org/openpgp.html).

When you confirmed that the public key is valid you can import it:

```
gpg --import public_key.asc
```

Note that if you see `no ultimately trusted keys found` it means you didn't create an OpenGPG key yet, which is not important for verifying a file.

## Download Public Key Using a Keyserver

If you have the fingerprint you can use the following command to import public key. Note that fingerprint may have space characters.

```
gpg --recv-keys [fingerprint]
```

Note that in some other tutorial they use something like `gpg --keyserver ...`. Since GnuPG 2.1 this option is deprecated and you should use `dirmgr`. Since all reputable keyservers are in sync, it's not very important to specify a keyserver. If you don't specify it, the default one is used. For more information read this Arch Wiki [page](https://wiki.archlinux.org/title/GnuPG#Configuration_files).

## Confirming the file

If you imported the keys before, it's important to refresh them to make sure none of them is revoked:

```
$ gpg --refresh-keys
```

Usually there should be a signature in ASCII armored format in download page. Using that you can verify the file:

```
gpg --verify-options show-notations --verify signature.asc file
```

Make sure the signature date make sense.
