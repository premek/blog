---
layout: post
title:  "Mounting NFS and Samba NAS in linux"
---

Trying to mount NFS and CIFS shares without the appropriate packages installed produces super obfuscated error messages.

<!--more-->

```
VFS: \\192.168.0.0\Share error -9 on ioctl to get interface list
```
```
VFS: cifs_mount failed w/return code = -13
```
So start with installing `cifs-utils` and `nfs-common` packages (this is on Debian).

Add entries to `/etc/fstab` for CIFS / Samba:
```
//192.168.0.1/Public   /mnt/public    cifs  defaults,_netdev,file_mode=0666,dir_mode=0777,credentials=/home/you/.smbpasswd 0 0
//192.168.0.1/ReadOnly /mnt/readonly  cifs  defaults,ro,_netdev,credentials=/home/you/.smbpasswd 0 0
```
Or for NFS:
```
192.168.0.1:/nfs/Public  /mnt/nfs-public   nfs  defaults,nosuid,_netdev,vers=3 0 0
```
After editing, run `systemctl daemon-reload`.

The `.smbpasswd` file with your NAS user and password:
```
username=franta
password=heslo
```

This will hopefully result in the NFS shares to be mounted on boot.
