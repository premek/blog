---
layout: post
title:  How to connect to a NTLMv2 share in Linux
---

If you just need to download a file you can use `smbclient` to connect to a NTLMv2 Windows share from Linux. You don’t need to edit `smb.conf` or any config file, just add the `-m SMB2` command line parameter.

```sh
$ smbclient -m SMB2 -U usernanme -W workgroup //server/service
```

For example:

```sh
$ smbclient -m SMB2 -U premek -W acme.local //10.20.30.40/public
```

And to download all files in a remote directory:

```sh
$ smbclient -m SMB2 -U premek -W acme.local //10.20.30.40/public \ 
 -c ‘prompt OFF;recurse ON;cd ‘path/to/remote/dir’;mget *’
```

