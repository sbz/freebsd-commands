# freebsd-commands

sbz's FreeBSD commands cheat-sheet

## Mount commands

- Mount MS-DOS file system (USB-stick, external FAT32 drive)

```
sudo mount_msdosfs [-o large] /dev/ad0s1 /mnt
```

- Mount ISO 9660 using memory disk

```
sudo mount -t cd9660 /dev/`mdconfig -f <image.iso>` /mnt
```

- Mount Linux procfs

```
sudo mkdir -p /proc
sudo mount -t procfs proc /proc
```

- Mount Linux linprocfs

```
sudo mkdir -p /compat/linux/proc
sudo mount -t linprocfs linproc /compat/linux/proc
```

- Mount file descriptor fs

```
sudo mount -t fdescfs fdesc /dev/fd
```

## Update commands

- Perform FreeBSD binary upgrade 

````
sudo freebsd-update fetch
sudo freebsd-update upgrade -r <release>
sudo freebsd-update install
````

## Pkg commands

- Update packages databases from repository

```
sudo pkg update
sudo pkg [-d] update
```

- Upgrade packages to new version

```
sudo pkg upgrade [-f]
```

- Update vuln.xml database

```
sudo pkg audit -F
```

- Is <pkg> installed?

```
pkg info|grep <pkg>
```

- Display package information

```
pkg info <pkg>
pkg show <pkg>
```

- Lock package to current version and Display locked packages

```
pkg lock <pkg>
pkg lock -l
```

- Clean local cache

```
sudo pkg clean -y
```

- Display packages stats

```
pkg stats
```

- Found the package installing the file:

```
pkg which /usr/local/bin/vim
```

- Found the file if package is not installed:

```
sudo pkg install pkg-provides
sudo pkg provides -uf
pkg provides /path/to/binary
```

## Network commands

- TCP open sockets (LISTEN, ESTABLISHED, CLOSE\_WAIT)

```
sudo netstat -p tcp -an
sudo sockstat -P tcp -a
```

## Kernel modules commands

- List loaded kernel modules

```
sudo kldstat [-v]
```

- Load Kernel module (HW thermal sensors)

```
sudo kldload <module>
sudo kldload coretemp
```

- Generate hints for the boot loader

```
sudo kldxref [v] /boot/kernel /boot/modules
sudo kldxref -R /boot
```

- Dump running kernel config

```
sysctl -n kern.conftxt
config -x /boot/kernel/kernel
```

## Ports commands

- Update and extract snapshot

```
sudo mkdir -p /usr/ports
sudo portsnap fetch extract
```

- Looking for a port in the tree

```
cd /usr/ports/*/*/<portname>
make -C /usr/ports search name=<portname>
make -C /usr/ports search name=<portname> display=name,path
psearch <portname>
```

- Display port variables

```
make -C /usr/ports/editor/vim -V MAINTAINER -V PORTVERSION
make -C /usr/ports/editor/vim -V WRKSRC -V WRKDIR
```

- Fetch distfile(s)

```
cd /usr/ports/editor/vim && make fetch extract
cd work
```

- Regenerate distfile(s) info hash

```
make -C /usr/ports/editor/vim makesum
```

- Alter KNOB/Options config

```
make -C /usr/ports/editor/vim showconfig
make -C /usr/ports/editor/vim config
```

## Poudriere commands

- Create jail

```
sudo poudriere jail -c -j <jail> -v 12.1-RELEASE -a <arch> -M ftp -p <ptree>
sudo poudriere jail -c -j 12amd64 -v 12.1-RELEASE -a amd64 -M ftp -p portsdir
```

- Delete jail

```
sudo poudriere jail -d -j <jail> -C all
sudo poudriere jail -d -j 12amd64 -C all
```

- List jail(s)

```
sudo poudriere jail -l
sudo poudriere jail -l [-n] [-q]
```

- Upgrade jail

```
sudo poudriere jail -u -j <jail>
sudo poudriere jail -u -j <jail> -t 12.1
```

- List ports tree

```
sudo poudriere ports -l
sudo poudriere ports -l [-n] [-q]
```

- Test port build

```
sudo poudriere testport -o <origin>/<port> -p portsdir -n # dry run
sudo poudriere testport -o editor/vim -p portsdir -v # verbose
```

## Developer commands

- Get sources

```
svn checkout [-q] https://svn.freebsd.org/base/head ~/svn/src
svn checkout [-q] svn://svn.freebsd.org/base/head ~/svn/src
svn checkout [-q] svn+ssh://svn.freebsd.org/base/head ~/svn/src
git clone --depth 1 https://github.com/freebsd/freebsd.git src
```

For specific 12.x release:

```
svn co [-q] svn+ssh://svn.freebsd.org/base/releng/12.1 ~/svn/src-12
```

- Get ports

```
svn checkout [-q] https://svn.freebsd.org/ports/head ~/svn/ports
svn checkout [-q] svn://svn.freebsd.org/ports/head ~/svn/ports
svn checkout [-q] svn+ssh://svn.freebsd.org/ports/head ~/svn/ports
git clone --depth 1 https://github.com/freebsd/freebsd-ports/git ports
```

- Enter into utils (e.g ls) sources code folder

```
cd `where -sq ls`
```

## Wireless command

- Restart wireless network

```
sudo service wpa_supplicant restart wlan0
```

- Debug wireless stack

```
sudo sysctl debug.iwi=1
sudo sysclt hw.wi.debug=1
sudo sysctl net.wlan.debug=1
```

## Build command

- World and Kernel build

```
cd /usr/src
sudo nice -n -20 make -j`sysctl -n hw.ncpu` -DNO_CLEAN -DKERNFAST buildworld buildkernel | tee -a build.log
```

- Install

```
cd /usr/src
sudo make installworld installkernel
sudo make installkernel.debug
```

- Update etc configs

```
sudo etcupdate
sudo mergemaster -ui
```
