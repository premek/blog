---
layout: post
title:  Configuring mvn deploy with svn+ssh
---


So just a few notes how I configured `mvn deploy` with `svn+ssh`. It may not work for you the same way it did for me but maybe it can help a bit.


# SSH Config
The repository URL in your configuration and the one of your repository has to be the same. So you don’t want your username in it. Put it in `~/.ssh/config` instead:

```
Host your_host_or_ip
 User your_svn_username
```

If you use both IP and hostname, put here both.

And do the clear checkout so you don’t see your name when doing `svn info`.

I use private key for ssh login to that host and no password.

# Maven config

This goes into the `pom.xml`. Replace `1.1.1.1` with your IP address or host. `/path/to/trunk/` is what you see in `svn info` but you also have to include any subdirectory of your project (the one that contains `pom.xml`).

```xml
<scm>
<connection>scm:svn:svn+ssh://1.1.1.1/path/to/trunk/and/project/</connection>
<developerConnection>scm:svn:svn+ssh://1.1.1.1/path/to/trunk/and/project/</developerConnection>
<url>scm:svn:svn+ssh://1.1.1.1/path/to/trunk/and/project/</url>
</scm>
```

`distributionManagement` defines where you deploy your artifacts to. Don’t forget to specify a repository with deploy permissions (e.g. libs-releases-local). You may need some additional config in `.m2/settings.xml` with your credentials.

```xml
<distributionManagement>
 <repository>
 <name>libs-releases-local</name>
 <url>http://1.1.1.1:2222/artifactory/libs-releases-local</url>
 <id>repo</id>
 </repository>
</distributionManagement>
```

# mvn release
Now, you prepare your release first, then perform, then realize you fucked it up, rollback, delete the svn tag (important!) and try again.

```sh
mvn release:prepare
mvn release:perform
mvn release:rollback
svn rm ^/tags/YourProject-3.30/ -m 'rollback tag'
```

If you need to rollback when you misconfigured your username, use `mvn -Dusername=yourusername release:rollback`

prepare, rollback, repeat:

![a screenshot from git log](/assets/2018-prepare-release.webp)
