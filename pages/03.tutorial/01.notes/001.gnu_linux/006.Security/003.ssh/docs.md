---
title: ssh
taxonomy:
    category: docs
---

## Managing multiple keys

Let's say we have server `example.com` and we defined two users `foo` and `bar` each of them has their own keys:

* Public keys: `foo_example_com.pub` and `bar_example_com.pub`
* Private keys: `foo_example_com` and `bar_example_com`

Then we create config file with the following content:

```
$ cat ~/.ssh/config
Match host=example.com
   IdentitiesOnly yes
   IdentityFile ~/.ssh/foo_example_com
   IdentityFile ~/.ssh/bar_example_com
```

For more information refer to this [wiki page](https://wiki.archlinux.org/title/SSH_keys#Managing_multiple_keys).

## Using `ssh-agent`

It keeps your private keys unencrypted for easy access. If you are using [Bourne shell (sh)](https://en.wikipedia.org/wiki/Bourne_shell) or one of its extension like Bash, you can run the agent by:

```
eval "$(ssh-agent -s)"
```

If you ignore `-s`, it tries to find out which shell you're using. Then you can add your private key:

```
ssh-add /path/to/private_key
```

Note that if you open a new shell, you may need to run the above commands again. For more information refer to [GitHub tutorial](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) and [Arch wiki](https://wiki.archlinux.org/title/SSH_keys#ssh-agent).

## Copying the public key to the remote server

You can use the following command. Note that public keys usually have `pub` extension.

```
ssh-copy-id -i /path/to/public_key remote_server
```

For more information refer to [Arch wiki](https://wiki.archlinux.org/title/SSH_keys#Copying_the_public_key_to_the_remote_server).

## Useful links

* [ssh keys](https://wiki.archlinux.org/title/SSH_keys)
