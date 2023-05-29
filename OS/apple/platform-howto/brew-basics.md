# Brew Basics

## Overview: 
Brew is a package manager for Mac's.  It allows you to install and keep up to date most open source code.  

## Very Basic things to do: 

### Update your local packages
The `update` command will update your local repo with the latest packages, but will not *instal* those packages.  

```
ansible: ~$ brew update


==> Downloading https://formulae.brew.sh/api/formula.jws.json
############################################################################################################### 100.0%
==> Downloading https://formulae.brew.sh/api/cask.jws.json
############################################################################################################### 100.0%
Already up-to-date.
ansible: ~$
```

### Looking for a package: 
The `search` command will look through the local repo for matches of packages to install.

```
ansible: ~$ brew search ansible
==> Formulae
ansible                       ansible-language-server       ansible@2.8                   ansible@6
ansible-cmdb                  ansible-lint                  ansible@2.9

==> Casks
ansible-dk
ansible: ~$
```

### Install something 
Here's how you would install wget: 

```$ brew install wget```

### Remove something
To remove or uninstall an app: 

```$ brew uninstall node```

### Cleanup
To remove old versions of packages, and free up used space: 

```$ brew cleanup```

## References: 
- [Homebrew: The Missing Package Manager for macOS (or Linux)](https://brew.sh/)
- [A Beginner’s Guide to Homebrew](https://medium.com/@kkworden/a-beginners-guide-to-homebrew-4b665956a74)