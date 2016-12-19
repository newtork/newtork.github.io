---
title:  "HowTo: Enlarge a Virtual Drive on VMware vSphere"
date:   2016-12-20 07:00:00 +0000
modified: 2016-12-20 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/20/howto-enlarge-virtual-drive-vmware/
categories: howto
tags: linux bash vmware howto
---

In the past we had to upgrade our *VMware vSphere* virtualization server and its virtualization hosts machine *VMware ESXi*. An old virtual debian machine has had multiple `dist-upgrade` iterations in its life and it even survived the *VMware* upgrade from *V4* to *V5.5*. Back then it was running an important OwnCloud instance and the virtual disk on this machine was running out of free space. That's when I was confronted with the virtual disk space enlargement.


<!--more-->

So no problem, you'd think, just open the properties of the machine instance, chose its virtual disk, change the size by entering a new number.

![screen][/content-images/vsphere-virtual-drive.jpg]

You might need to let the operating system refresh it's filesystem partition table; first check, then resize:

```
e2fsck -f /dev/sdb1
resize2fs /dev/sdb1
```

You should be good to go from now on.


### Manual changes

If you are out of luck, the *virtual machine* or *virtual drive* does not support enlarging or shrinking. The field is grayed out in this case. That's when you need to create a new *virtual drive* of custom (bigger) size, copy the data, remount and register it in the file system table.





Dieser ganze manuelle Konfigurationskram ist nicht mehr notwendig, wenn es auch dynamisch/automatisch geht.





How to Expand Owncloud server file system manually:


0. set applications to *maintenance mode*, unmount the old partition
vi /var/www/owncloud/config/config.php

 
1. Create new partition
fdisk /dev/sdb


2. Copy FROM sdc1 (old,small) TO sdb1 (new,big), blocksize (bs) is adjustable

this shell starts the copying process from partition `sdc1` of small drive `sdc` to big partition `dcb1` of drive `sdb1` as a background process. The background process identifier is saved as `$ddpid`, to which every 5 seconds a *SIGUSR1* signal is being sent. That signal tells a process to *softly* terminate by default. While copying `dd` will ignore the signal. This loops until the process identified by `$ddpid` is finally terminated.

**Note:** `dd` sometimes stays a running state after completion, just waiting for such a stop signal.


```
dd if=/dev/sdc1 of=/dev/sdb1 bs=10M & ddpid=$! ; while [ $(ps -ao pid | grep $ddpid) ]; do kill -SIGUSR1 $ddpid; sleep 5; done
```
 

{% include tags/hint-start.html %}
**Usefull commands**

 - Show drive / partitions list
 
```fdisk -l``` 

 - Show mounted partitions

```df –h```

 - Show drive UUID list

```blkid```
{% include tags/hint-end.html %}



 

4. Fix UUID of sdb1 (new,big, set a random one, or write a custom one  like `"c1b9d5a2-f162-11cf-9ece-0020afc76f16"`)
tune2fs /dev/sdb1 -U random

 

5. File system check of sdb1
e2fsck -f /dev/sdb1

 

6. Expand sdb1 file system to correct size, automatically from “small” to “big”
resize2fs /dev/sdb1

 

7. Check status after mounting
mkdir /owncloud.clone
mount /dev/sdb1 /owncloud.clone
df -h

 

8. Fix partition startup routine, use UUID previously generated
vi /etc/fstab
reboot



 