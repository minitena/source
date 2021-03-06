# Contributing

## Introduction
Welcome to this page, Minitena's contributors! I'm [protonesso](https://github.com/protonesso) the creator of Minitena. At this page, you can find information about Minitena contribution.

## Preparing


### Building tools for ```wok```
```wok``` is a framework that builds Minitena. It can build cross-toolchain, chroot system, tarball and *.iso image. ```wok``` uses ```pacman``` as package manager to manipulate chroot and toolchain packages on host system and native packages on the target system. First, we'll install host packages.

Debian or Ubuntu (and derivatives):
```
apt-get install build-essential m4 wget gawk bc bison flex texinfo python3 python perl libtool autoconf automake autopoint gperf bsdtar libarchive-dev fakeroot xorriso curl syslinux-utils squashfs-tools pkg-config libssl-dev libcurl4-openssl-dev
```
Fedora (and derivatives):
```
dnf install fakeroot libarchive-devel libarchive bsdtar openssl-devel curl-devel autoconf automake git autoconf automake gawk m4 bison flex texinfo patchutils gcc gcc-c++ libtool gettext-devel squashfs-tools xorriso glibc-static
```
Arch Linux (and derivatives):
```
pacman -S base-devel syslinux xorriso squashfs-tools
```
Minitena:
```
pacman -S meta-base syslinux xorriso squashfs-tools
```
Building pacman:
```
cd /tmp
wget https://sources.archlinux.org/other/pacman/pacman-5.1.1.tar.gz
tar -xvf pacman-5.1.1.tar.gz
cd pacman-5.1.1
sed -i -e '/x-cpio/s@)@|*application/x-empty*)@' scripts/makepkg.sh.in
sed -i -e 's/EUID == 0/EUID == -1/' scripts/makepkg.sh.in
./configure \
--prefix=/dedicated/dir \
--localstatedir=/var \
--with-scriptlet-shell=/bin/bash \
--disable-doc \
--disable-nls \
DUFLAGS="-sk" \
SEDINPLACEFLAGS="-i"
make -j $(expr $(nproc) + 1)
make install -j $(expr $(nproc) + 1)
```
After building ```pacman``` you need to add path to /dedicated/dir/bin in your bashrc


### Building Minitena
The first step is building chroot system for Minitena. Minitena supports 4 CPU architectures:
```
x86_64  - 64-bit x86 (AMD64/EMT64)
i586    - 32-bit x86 (for Pentium-compatible and higher)
aarch64 - 64-bit ARM
arm     - 32-bit ARM (version 7)
```
Building chroot system:
```
sudo BARCH=[CPU architecture] MKJOBS=[number of CPU cores (if you don't want to specify that wok will use all of your CPU cores!)] ./wok bootstrap
```
**WARNING: If you want to build ARM system on x86 you should use enter-chroot-binfmt!**
Then we need to enter chroot:
```
sudo BARCH=[CPU architecture] ./wok enter-chroot
```
Let's build core system:
```
wok core
```
After that all you can build *.iso image (x86_64 and i586 only):
```
sudo BARCH=[CPU architecture] ./wok image
```
And *.tar.xz tarball (all architectures)
```
sudo BARCH=[CPU architecture] ./wok tarball
```


## Making packages
TODO
