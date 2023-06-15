# Brew Basics

**Table Of Contents**
- [Brew Basics](#brew-basics)
  - [Overview:](#overview)
  - [Updating what you currently have:](#updating-what-you-currently-have)
    - [Update your local packages](#update-your-local-packages)
    - [Upgrade all your installed packages](#upgrade-all-your-installed-packages)
    - [Cleanup](#cleanup)
  - [Installing new things](#installing-new-things)
    - [Looking for a package:](#looking-for-a-package)
    - [Install something](#install-something)
  - [Additional Housecleaning](#additional-housecleaning)
    - [View outdated packages](#view-outdated-packages)
    - [Remove something](#remove-something)
  - [References:](#references)


## Overview: 
Brew is a package manager for Mac's.  It allows you to install and keep up to date most open source code.  

## Updating what you currently have: 

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

### Upgrade all your installed packages
The `upgrade` command updates all installed packages on your machine.

```
~: ~$ brew upgrade
==> Downloading https://formulae.brew.sh/api/formula.jws.json

==> Upgrading 12 outdated packages:
git-lfs 3.2.0 -> 3.3.0
faad2 2.10.0 -> 2.10.1
imlib2 1.9.1_1 -> 1.11.1
[...blablabla...]
```

### Cleanup
To remove old versions of packages, and free up used space: 

```$ brew cleanup```

## Installing new things

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

## Additional Housecleaning

### View outdated packages
See what you have that needs to be updated

```
 ~ $ brew outdated
ansible (2.9.11) < 2.10.3_1
ansible-lint (4.3.0) < 4.3.7
aom (2.0.0) < 2.0.1
git (2.28.0) < 2.29.2
git-lfs (2.12.0) < 2.12.1
glib (2.64.5) < 2.66.2_1
gmp (6.1.2_2, 6.2.0) < 6.2.1
gobject-introspection (1.64.1_2) < 1.66.1_1
highlight (3.57) < 3.59_1
iproute2mac (1.3.0) < 1.3.0_1
libdnet (1.12) < 1.14
libressl (3.1.4) < 3.2.2
lua (5.3.5_1) < 5.4.2
mtr (0.93_1) < 0.94
mysql-client (8.0.21) < 8.0.22
nmap (7.80_1) < 7.91
ntopng (4.0_1) < 4.2_1
pango (1.46.1) < 1.48.0
pyenv (1.2.20) < 1.2.21
python@3.8 (3.8.5) < 3.8.6_2
redis (6.0.8) < 6.0.9
sphinx-doc (3.2.1) < 3.3.1_1
unbound (1.11.0) < 1.12.0_1
scribus (1.5.5) != 1.5.6.1
cmercier@pine ~ $
```

### Remove something
To remove or uninstall an app: 

```$ brew uninstall node```

## References: 
- [Homebrew: The Missing Package Manager for macOS (or Linux)](https://brew.sh/)
- [A Beginner’s Guide to Homebrew](https://medium.com/@kkworden/a-beginners-guide-to-homebrew-4b665956a74)