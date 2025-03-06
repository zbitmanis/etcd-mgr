# Information about the node  
ec2-3-64-8-66.eu-central-1.compute.amazonaws.com
## General node information
- Node arch:  

  x86_64
- Distribution 

  Ubuntu -  22.04 

- Hostname 
  
  ip-10-0-1-15

- Host FQDN 
  
  ip-10-0-1-15.eu-central-1.compute.internal

- Node root-device:

  PARTUUID=895d8984-5441-4c70-b87c-a6b6ebb8c95e

## Network information

- Network interfaces -  OS level

  eth1 - mac: 02:30:9b:6d:9d:8d ipv4: 10.0.2.251 active: True 

  eth0 - mac: 02:34:bb:c8:55:3d ipv4: 10.0.1.15 active: True 

 

- Network interfaces - Cloud provider

  eth1 - mac: 02:30:9b:6d:9d:8d ipv4: 10.0.2.251 

  eth0 - mac: 02:34:bb:c8:55:3d ipv4: 10.0.1.15 

 

## Name resolution related settings 

### NS resolution - OS level 

- /etc/hosts

```
>cat /etc/hosts 


127.0.0.1 localhost

# The following lines are desirable for IPv6 capable hosts
::1 ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts
 
```

- /etc/resolv.conf

```
>cat /etc/resolv.conf 


# This is /run/systemd/resolve/stub-resolv.conf managed by man:systemd-resolved(8).
# Do not edit.
#
# This file might be symlinked as /etc/resolv.conf. If you're looking at
# /etc/resolv.conf and seeing this text, you have followed the symlink.
#
# This is a dynamic resolv.conf file for connecting local clients to the
# internal DNS stub resolver of systemd-resolved. This file lists all
# configured search domains.
#
# Run "resolvectl status" to see details about the uplink DNS servers
# currently in use.
#
# Third party programs should typically not access this file directly, but only
# through the symlink at /etc/resolv.conf. To manage man:resolv.conf(5) in a
# different way, replace this symlink by a static file or a different symlink.
#
# See man:systemd-resolved.service(8) for details about the supported modes of
# operation for /etc/resolv.conf.

nameserver 127.0.0.53
options edns0 trust-ad
search eu-central-1.compute.internal
 
```

- /etc/nsswitch.conf

```
>cat /etc/nsswitch.conf 


# /etc/nsswitch.conf
#
# Example configuration of GNU Name Service Switch functionality.
# If you have the `glibc-doc-reference' and `info' packages installed, try:
# `info libc "Name Service Switch"' for information about this file.

passwd:         files systemd
group:          files systemd
shadow:         files
gshadow:        files

hosts:          files dns
networks:       files

protocols:      db files
services:       db files
ethers:         db files
rpc:            db files

netgroup:       nis
 
```

 

## Block device info
- Block devices  -  OS level 

```
>lsblk -af --raw -o +PARTUUID


NAME FSTYPE FSVER LABEL UUID FSAVAIL FSUSE% MOUNTPOINTS PARTUUID
loop0 squashfs 4.0   0 100% /snap/amazon-ssm-agent/6312 
loop1 squashfs 4.0   0 100% /snap/core18/2632 
loop2 squashfs 4.0   0 100% /snap/core20/1695 
loop3 squashfs 4.0   0 100% /snap/snapd/17883 
loop4 squashfs 4.0   0 100% /snap/lxd/23541 
loop5 squashfs 4.0   0 100% /snap/snapd/23545 
loop6 squashfs 4.0   0 100% /snap/core18/2855 
loop7 squashfs 4.0   0 100% /snap/lxd/31333 
loop8 squashfs 4.0   0 100% /snap/core20/2496 
loop9 squashfs 4.0   0 100% /snap/core22/1748 
loop10 squashfs 4.0   0 100% /snap/amazon-ssm-agent/9881 
xvda        
xvda1 ext4 1.0 cloudimg-rootfs 687fab62-1ba5-4282-890e-9266064f7d27 4.9G 35% / 895d8984-5441-4c70-b87c-a6b6ebb8c95e
xvda14        254baa1e-1b35-41e3-90db-935a8367ac19
xvda15 vfat FAT32 UEFI B2B4-82AC 98.3M 6% /boot/efi 0cf1c52c-98f5-48ae-8a07-fff782190e30
 
```
- Partitions - OS level

```
> fdsik -l

Disk /dev/loop0: 24.39 MiB, 25571328 bytes, 49944 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop1: 55.58 MiB, 58281984 bytes, 113832 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop2: 63.24 MiB, 66314240 bytes, 129520 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop3: 49.62 MiB, 52031488 bytes, 101624 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop4: 102.98 MiB, 107986944 bytes, 210912 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop5: 44.44 MiB, 46596096 bytes, 91008 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop6: 55.36 MiB, 58052608 bytes, 113384 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop7: 89.4 MiB, 93745152 bytes, 183096 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/xvda: 8 GiB, 8589934592 bytes, 16777216 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: B28BC60D-C864-49DB-A9FF-7B6FF9BCC625

Device       Start      End  Sectors  Size Type
/dev/xvda1  227328 16777182 16549855  7.9G Linux filesystem
/dev/xvda14   2048    10239     8192    4M BIOS boot
/dev/xvda15  10240   227327   217088  106M EFI System

Partition table entries are not in disk order.


Disk /dev/loop8: 63.75 MiB, 66842624 bytes, 130552 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop9: 73.89 MiB, 77479936 bytes, 151328 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop10: 26.32 MiB, 27602944 bytes, 53912 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
 

```
- NVMe devices - OS level

```
> nvme list

Node                  SN                   Model                                    Namespace Usage                      Format           FW Rev  
--------------------- -------------------- ---------------------------------------- --------- -------------------------- ---------------- --------
 

```
 
 
- Block device mapping - Cloud provider


  ami - mapping: sda1 metadata url : http://169.254.169.254/latest/meta-data/block-device-mapping/ami 

  ephemeral0 - mapping: sdb metadata url : http://169.254.169.254/latest/meta-data/block-device-mapping/ephemeral0 

  ephemeral1 - mapping: sdc metadata url : http://169.254.169.254/latest/meta-data/block-device-mapping/ephemeral1 

  root - mapping: /dev/sda1 metadata url : http://169.254.169.254/latest/meta-data/block-device-mapping/root 

 