---
layout: post
title:  "Docker: Domain Controller with Samba"
date:   2016-12-18 07:00:00 +0000
modified: 2016-12-18 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/18/docker-domain-controller-with-samba/
categories: project 
tags: docker project windows domain groupware
---

I worked some time in a company environment traditionally build around Microsoft's *Active Directory* and I liked the user and group management, even the administration of their privilege setttings. We ran several virtual *Debian* machines to host webservices, which authenticated their users via *LDAP* and *Kerberos*. Luckily as developers, we do not need to purchase any Microsoft product for building or testing. All you need is a *Domain Controller* docker image.


<!--more-->

As I needed to configure different content management systems, cloud storage, webspace and groupware solutions, I wanted them to use the same *Active Directory* connection, which could be served from a classic Microsoft domain controller. But here is an interesting, yet unstable, alternative: [Samba][samba] + [Bind9][bind9] can be integrated and serve as Active Directory with DNS server. This is useful when testing authentication on various software. In my [groupware collection][github-main] you will find the new [Domain Controller docker image][github-domain]. You can download it from [Docker Hub][dockerhub].



{% include tags/hint-start.html %}
Docker containers are not meant to host such critical company infrastructure! Consider this docker setup ignored and always use at least **one dedicated server** when building a *Domain Controller* in production.
{% include tags/hint-end.html %}


Anyways, if you want to give this image a try, download it either from [DockerHub][dockerhub]...

```
docker pull newtork/groupware-domain
```

... or download it from [GitHub][github-domain] and build it yourself:

```
docker build -t newtork/groupware-domain .
```



## Requirements

Let's first face the constraints for Samba4 Active Directory:

 - partition filesystem with support for ACL and extended attributes.
 - propagation between two instances is not possible out of the box.

These issues significantly increase the complexity of this docker build.

### Local Host Directory

To run this docker you will need to

 1. have a dedicated partition or path on your host machine, which will later keep all *Active Directory* data. We are talking about a *hundred MB* or so. Use `fdisk` in case you want to create a new partition.
 1. make sure, this dedicated partition or path supports ACL / xattr. If not, format it with *EXT4* with `mkfs.ext4`, this will have both options activated by default.

To check *ACL* / *xattr* support on your local partition (e.g. `sda1` with `ext4`), use the following command:


{% assign shell-types = "user output" %}
{% include tags/shell-ind.html %}
```
grep 'xattr\|acl' /proc/fs/ext4/sda1/options
user_xattr
acl
```

## Start

There are multiple options to start the image with. We'll go into depth as the tutorial goes on. Let's assume you mounted the dedicated partition and/or created a suitable path as `/var/domain/`.



{% include tags/hint-start.html %}
When testing different domain setups, I would recommend cleaning your working directory before each try: `rm -rf /var/domain/*`
{% include tags/hint-end.html %}



To get started, let's do a test run. We can start it up without additional paramters. The following command will create a *domain* from the build's defaults:

```
docker run --cap-add SYS_ADMIN --volume /var/domain:/data --rm -it newtork/groupware-domain
```

Explanation:

 - Unfortunately the container needs `SYS_ADMIN` capabilities to run the *Samba* domain provisioning with *extended attributes*. While xattr features by themselves are working fine even without elevated privileges, there seems to be a dependency for users and file ownership, which make `SYS_ADMIN` a requirement.
 - If you want to **skip** the `SYS_ADMIN` requirement you would need to give up on the *extended attributes*, which would be unfortunate and not recommended, but still possible.

```
docker run --volume /var/domain:/data --rm -it newtork/groupware-domain --no-xattr
```

### Help

To view a full set of arguments, see the `help`:

```
docker run --rm -it newtork/groupware-domain --help
```

As you can see, most options are also configurable as arguments during `docker run`. To get a better idea of possible startup configurations, you can run the interactive mode, which guides you through everything:

```
docker run --volume /var/domain:/data --rm -it newtork/groupware-domain --interactive
```


### Example

There are some port forwardings, which need to be set in order get a functioning *domain controller* on your local host.

```
docker run \
--cap-add SYS_ADMIN \
--hostname=dc01 \
--name=dc01 \
--volume /var/domain:/data \
--rm \
-it \
-p 53:53 -p 53:53/udp \
-p 88:88 -p 88:88/udp \
-p 135:135 \
-p 137:137/udp \
-p 138:138/udp \
-p 139:139 \
-p 389:389 -p 389:389/udp \
-p 445:445 \
-p 464:464 -p 464:464/udp \
-p 636:636 \
-p 1024-1152:1024-1152 \
-p 3268:3268 \
-p 3269:3269 \
-p 22137:22137 \
newtork/groupware-domain
```

This is the port usage for *Samba`s active directory*:


| Service | Port | Protocol |
|---|---|---|
| DNS | 53 | tcp/udp |
| Kerberos | 88 | tcp/udp |
| End Point Mapper | 135 | tcp |
| NetBIOS Name Service | 137 | udp |
| NetBIOS Datagram | 138 | udp |
| NetBIOS Session | 139 | tcp |
| LDAP | 389 | tcp/udp |
| SMB over TCP | 445 | tcp |
| Kerberos kpasswd | 464 | tcp/udp |
| LDAPS | 636 | tcp |
| Dynamic RPC Ports | 1024-5000* | tcp |
| Global Cataloge | 3268 | tcp |
| Global Cataloge SSL | 3269 | tcp |



{% include tags/hint-start.html %}
Find more feature by following the tags above, e.g. [groupware][more]. There are some interesting topics like *domain replication* on *secondary domain controller*, user *smb shares*, service *LDAP usage* and more! Also please mind the *Docker Compositions* provided.
{% include tags/hint-end.html %}



[bind9]: https://wiki.debian.org/Bind9
[samba]: https://wiki.samba.org/index.php/Main_Page
[more]: /tag/groupware/
[github-main]: https://github.com/newtork/docker-groupware
[github-domain]: https://github.com/newtork/docker-groupware-domain
[dockerhub]: https://hub.docker.com/r/newtork/groupware-domain/