# Brew



## Update 
```bash
cmercier@pine ~ $ brew update
Updated 1 tap (homebrew/cask).
==> New Casks
zeitgeist
==> Updated Casks
aerial
cmercier@pine ~ $
```

```bash
cmercier@pine ~ $ brew outdated
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

```bash
cmercier@pine ~ $ brew upgrade
==> Upgrading 23 outdated packages:
git-lfs 2.12.0 -> 2.12.1
pyenv 1.2.20 -> 1.2.21
gmp 6.2.0 -> 6.2.1
mysql-client 8.0.21 -> 8.0.22
iproute2mac 1.3.0 -> 1.3.0_1
redis 6.0.8 -> 6.0.9
highlight 3.57 -> 3.59_1
pango 1.46.1 -> 1.48.0
mtr 0.93_1 -> 0.94
glib 2.64.5 -> 2.66.2_1
aom 2.0.0 -> 2.0.1
ansible-lint 4.3.0 -> 4.3.7
gobject-introspection 1.64.1_2 -> 1.66.1_1
lua 5.3.5_1 -> 5.4.2
ntopng 4.0_1 -> 4.2_1

[... remove excess ...]

==> Upgrading scribus
==> Downloading https://downloads.sourceforge.net/scribus/scribus-devel/1.5.6.1/
==> Downloading from https://netactuate.dl.sourceforge.net/project/scribus/scrib
######################################################################## 100.0%
==> Backing App 'Scribus.app' up to '/usr/local/Caskroom/scribus/1.5.5/Scribus.a
==> Removing App '/Applications/Scribus.app'.
==> Moving App 'Scribus.app' to '/Applications/Scribus.app'.
==> Purging files for version 1.5.5 of Cask scribus
🍺  scribus was successfully upgraded!
cmercier@pine ~ $
```

