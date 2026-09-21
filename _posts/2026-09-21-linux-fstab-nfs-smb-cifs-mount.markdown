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
//192.168.0.1/Public   /mnt/public    cifs  defaults,noatime,nosuid,file_mode=0666,dir_mode=0777,credentials=/home/you/.smbpasswd,_netdev,nofail,x-systemd.automount 0 0
//192.168.0.1/ReadOnly /mnt/readonly  cifs  defaults,ro,credentials=/home/you/.smbpasswd,_netdev,nofail,x-systemd.automount 0 0
```
Or for NFS:
```
192.168.0.1:/nfs/Public  /mnt/nfs-public   nfs  defaults,nosuid,vers=3,_netdev,nofail,x-systemd.automount 0 0
```
After editing, run `systemctl daemon-reload`.

The `.smbpasswd` file with your NAS user and password:
```
username=franta
password=heslo
```

The automount option should result in the network shares to be mounted on the first access and not block boot in case it's not connected.

