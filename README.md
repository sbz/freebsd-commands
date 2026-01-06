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

- Find the package installing the file:

```
pkg which /usr/local/bin/vim
```

- Find the file if package is not installed:

```
sudo pkg install pkg-provides
sudo pkg provides -uf
pkg provides /path/to/file
```

## Network commands

- TCP open sockets (LISTEN, ESTABLISHED, CLOSE\_WAIT)

```
sudo netstat -p tcp -an
sudo sockstat -P tcp -a
```

Also refer to dtrace tcp script into /usr/share/dtrace

```
cd /usr/share/dtrace
sudo tcpconn
sudo tcpdebug
sudo tcpstate
sudo tcptrack
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

sudo pkg install psearch
psearch <portname>
```

- Display port compiler flags

```
make -C /usr/ports/editors/vim -V CFLAGS
```

- Display port variables

```
make -C /usr/ports/editors/vim -V MAINTAINER -V PORTVERSION
make -C /usr/ports/editors/vim -V WRKSRC -V WRKDIR
```

- Fetch distfile(s)

```
cd /usr/ports/editors/vim && make fetch extract
cd work
```

- Regenerate distfile(s) info hash

```
make -C /usr/ports/editors/vim makesum
```

- Alter KNOB/Options config

```
make -C /usr/ports/editors/vim showconfig
make -C /usr/ports/editors/vim config
make -C /usr/ports/editors/vim rmconfig

make check-license check-categories check-deprecated check-vulnerable security-check check-sanity check-plist check-orphans check-config
```

- List ports Makefile targets

```
grep -E '^[^${\.#]+:$' /usr/ports/Mk/bsd.port.mk |cut -d ':' -f1 | sort -u
make -C /usr/ports -V .ALLTARGETS
```

- List dependencies to rebuild

```
make all-depends-list
make build-depends-list
make run-depends-list
```

- Rebuild ports w/o building their dependencies

```
make missing-packages # list missing packages
make install-missing-packages
make install clean
```


## Src commands

- Extract /usr/src Makefile targets with descriptions (list all available targets)

```
grep '^# [a-z].*- [A-Z].*' /usr/src/Makefile | sed 's,^# ,,' | sort
make -C /usr/src -V .ALLTARGETS
```

- Enter into userland binary utility (e.g ls) sources code folder

```
cd `whereis -sq ls`
```

## Poudriere commands

- Create jail

```
sudo poudriere jail -c -j <jail> -v 15.0-RELEASE -a <arch> -m ftp -p <ptree>
sudo poudriere jail -c -j 15amd64 -v 15.0-RELEASE -a amd64 -m ftp -p portsdir
```

- Delete jail

```
sudo poudriere jail -d -j <jail> -C all
sudo poudriere jail -d -j 15amd64 -C all
```

- List jail(s)

```
sudo poudriere jail -l
sudo poudriere jail -l [-n] [-q]
```

- Upgrade jail

```
sudo poudriere jail -u -j <jail>
sudo poudriere jail -u -j <jail> -t 15.0
```

- Create ports

```
sudo poudriere ports -c -m null -M ${PWD}/git/ports -p portsdir -v
sudo poudriere ports -l
```

- List ports tree

```
sudo poudriere ports -l
sudo poudriere ports -l [-n] [-q]
```

- Test port build

```
sudo poudriere testport -j <jail> -o <origin>/<port> -p portsdir -n # dry run
sudo poudriere testport -j <jail> -o editors/vim -p portsdir -v # verbose
```

## Developer commands

- Get sources

_(via subversion: deprecated)_
```
svn checkout [-q] https://svn.freebsd.org/base/head ~/svn/src
svn checkout [-q] svn://svn.freebsd.org/base/head ~/svn/src
svn checkout [-q] svn+ssh://svn.freebsd.org/base/head ~/svn/src
```

*(via git)*
```
git clone --depth 1 https://github.com/freebsd/freebsd.git /usr/src
git clone --depth 1 https://git.freebsd.org/src.git /usr/src
```

For specific branch, e.g. 14.x release:

```
git checkout -b releng-14.3 freebsd/releng/14.3
git switch -c releng-14.3 freebsd/releng/14.3
```

- Get ports

_(via subversion: deprecated)_
```
svn checkout [-q] https://svn.freebsd.org/ports/head ~/svn/ports
svn checkout [-q] svn://svn.freebsd.org/ports/head ~/svn/ports
svn checkout [-q] svn+ssh://svn.freebsd.org/ports/head ~/svn/ports
```

*(via git)*
```
git clone --depth 1 https://github.com/freebsd/freebsd-ports.git /usr/ports
git clone --depth 1 https://git.freebsd.org/ports.git /usr/ports
```

## Wireless commands

- Restart wireless network

```
sudo service wpa_supplicant restart wlan0
```

- List Wireless devices

```
sysctl net.wlan.devices
```

- List Wireless SSID Access point (w/ wlan0 device)

```
sudo ifconfig [-v] wlan0 list scan
```

- Debug wireless stack (e.g. case for iwn(4) driver)

```
sudo sysctl debug.iwi=1
sudo sysctl hw.wi.debug=1
sudo sysctl net.wlan.debug=1
```

## Bluetooth commands

- Scan Bluetooth peripherals

```
sudo bluetooth-config scan
```

- Enable Bluetooth association

```
sudo service hcsecd start
```

- List Bluetooth connections

```
sudo btsockstat -n
```

## Build commands

- World and Kernel build

```
cd /usr/src
sudo nice -n -20 make -j`sysctl -n hw.ncpu` -DNO_CLEAN -DKERNFAST buildworld buildkernel | tee -a build.log
```

- Install Kernel (debug)

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

## Hardware commands

- PCI devices

```
sudo pciconf -vl

sudo pkg install pciutils
sudo lspci [-v]
```

- USB devices

```
sudo usbconfig list
sudo usbconfig dump_all_desc

sudo pkg install usbutils
sudo lsusb [-v]
```

- CPU Info

```
sudo dmesg
sudo dmesg | sed -n '/^CPU:/,/^real/p'
sudo sysctl hw.model hw.ncpu
sudo sysctl kern.smp.cpus
```

## Memory commands

- Virtual memory statistics

```
vmstat -c 1
sysctl hw.realmem hw.physmem
top -bt 0
```

- Memory total, wired, active, cache

```
sysctl vm.stats|grep count
```

- Process memory mappings

```
procstat vm <pid> or procstat -v <pid>
cat /proc/<pid>/map
cat /compat/linux/proc/<pid>/maps
```

## Sounds commands

- Sounds devices

```
sudo cat /dev/sndstat
sudo sysctl dev.pcm
```

- Disable beep

```
sudo sysctl hw.syscons.bell=0
sudo sysctl kern.vt.enable_bell=0
```

- Volume mixer

```
mixer vol 100
```

## IO commands

- Device read/write IO stats

```
iostat [-x]
iostat -x -w 1 # watch mode
```

## ZFS Commands

- ZFS Take snapshot (of zroot)

```
sudo zfs snapshot -r zroot@<name-of-snapshot>
```

- ZFS List snapshot

```
zfs list -t snapshot
ls -1 /.zfs/snapshot/
```

- ZFS Pools import 

```
sudo zpool import -R /mnt zroot
sudo zpool import -R /mnt -e readonly=on zroot # readonly
```

- ZFS Datasets list and mount

```
zfs list
sudo mount -t vfs zroot/usr/home /tmp/home
```

- ZFS Protect snapshot from deletion

```
sudo zfs hold keep -r zroot@<name-of-snapshot>
zfs holds zroot@<name-of-snapshost>
```

- ZFS Restore snapshot

```
sudo zfs rollback zroot@<name-of-snapshot>
```

- ZFS Destroy snapshot

```
sudo zfs destroy -r zroot@<name-of-snapshot>
```
