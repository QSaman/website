---
title: GPG
taxonomy:
    category: docs
---

## GPG vs. PGP vs. OpenPGP

[GNU Privacy Guard (GPG)](https://en.wikipedia.org/wiki/GNU_Privacy_Guard) is available out of the box in most GNU/Linux Distros. Both GPG and the proprietary [Pretty Good Privacy](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) implement OpenPGP standard.

## Listing Current Fingerprints

To see the current [public key fingerprints](https://en.wikipedia.org/wiki/Public_key_fingerprint) run the following command:

```
gpg --fingerprint
```

# Verifying Integrity of a File

First you need to download the public key from the website that has `asc` extension. It's a public key in Base64 format which is called `ASCII armor` file in OpenGPG [Standard](https://en.wikipedia.org/wiki/Base64#OpenPGP).

You can check fingerprint of the public key without importing it by running this command:

```
gpg --keyid-format long --import --import-options show-only --with-fingerprint public_key.asc
```

Then check the output and make sure fingerprint is the one that you expect. If you see a message like `signatures not checked due to missing keys` then you can use [OpenGPG Web of Trust](https://en.wikipedia.org/wiki/Web_of_trust) to remove it. For example you can import the public key of a well-known Debian developer like Jan Dittberner from Debian keyring [website](https://keyring.debian.org/) using the following command:

```
gpg --keyserver keyring.debian.org --recv-keys B2FF1D95CE8F7A22DF4CF09BA73E0055558FB8DD
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


Now that we import the public key, it's time to verify the file itself. Usually there should be a signature in ASCII armored format in download page. Using that you can verify the file:

```
gpg --verify-options show-notations --verify signature.asc file
```

Make sure the signature date make sense.
