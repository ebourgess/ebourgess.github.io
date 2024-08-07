---
layout: post
title:  "[Git] Signing Commits"
date:   2022-05-31
slug: "signing-git-commits"
authors: ["Elias Bourgess"]
categories: ["DevOps", "GitOps"]
draft: false
---

This document serves as a step by step guide to how to sign commits before pushing to the repository in Github.

## Generating a GPG Key 

In order to generate a `gpg` key, simply run the following command:

```shell
gpg --full-generate-key
```

It will prompt you for the following:
- Specify the key you want - Press `Enter` to accept the default
- Specify the key size you want - Press `Enter` to accept the default value 
- Specify the time the key would be valid for - Press `Enter` to specify that the key doesn't expire
- Enter Your user ID information
- Type a secure passphrase - Press `Enter`  for an empty passphrase

After finishing with the prompt, check if your key is there: 

```sh
gpg --list-secret-keys --keyid-format=long
```

Example output for the above would be 

```shell
sec   rsa3072/XXXXXXXXXXXXXXXX 2022-05-26 [SC]
      XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
uid                 [ultimate] gpg-key-github (gpg key for github) <email>
ssb   rsa3072/XXXXXXXXXXXXXXXX 2022-05-26 [E]
```

To extract your PGP key simply run, while replacing `XXXXXXXXXXXXXXXX` with the actual ID that you got from command below:

```shell
gpg --armor --export XXXXXXXXXXXXXXXX
```

The output would be similar to the following below

```shell
-----BEGIN PGP PUBLIC KEY BLOCK-----

....

-----END PGP PUBLIC KEY BLOCK-----
```

## Add the GPG key to Github 

In order to add the pgp key to your Github Account copy the output from above, then

1. Go to your Github Account
2. Click on Settings 
3. In the `Access` part of the section click on   **SSH and GPG keys**.
4. Then add new GPG key 
5. Paste the output from above and click on `Add GPG key`


## Configure Git to use your signing key

To Configure Git to use your signing key, simply run the following command: 

```shell
git config --global user.signingkey XXXXXXXXXXXXXXXX
```

If you are using `bash` you should add the following to your `.bashrc` 

```shell
if [ -r ~/.bash_profile ]; then echo 'export GPG_TTY=$(tty)' >> ~/.bash_profile; \
  else echo 'export GPG_TTY=$(tty)' >> ~/.profile; fi
```

Alternatively, if you are using `zsh` you should add the following to `.zshrc` , or if it exists, you `.zprofile`

```shell
if [ -r ~/.zshrc ]; then echo 'export GPG_TTY=$(tty)' >> ~/.zshrc; \
  else echo 'export GPG_TTY=$(tty)' >> ~/.zprofile; fi
```

## Signing Commits

Simply run the following command: 

```shell
git commit -S -m "your commit message"
```

Then push changes to your remote repository

```shell
git push
```

## Signing tags

```shell
git tag -s mytag
# Creates a signed tag
```

```shell
git tag -v mytag
# Verifies the signed tag
```