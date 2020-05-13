# freebsd-commands

sbz's FreeBSD commands cheat-sheet

## Mount commands

- Mount MS-DOS file system (USB-stick, external FAT32 drive)

```
sudo mount_msdosfs [-o large] /dev/ad0s1 /mnt
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

- Found what package install the file:

```
pkg which /usr/local/bin/vim
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
make -C /usr/ports search name=<portname> display=name,category
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
sudo poudriere testport -o editor/vim -p portsdir -n
sudo poudriere testport -o editor/vim -p portsdir -v
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

## Wireless command

```
sudo service wpa_supplicant restart wlan0
```
