---
layout: post
title:  "HowTo: Fix out-dated OwnCloud directory cache"
date:   2016-12-13 07:00:00 +0000
modified: 2016-12-13 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/13/howto-fix-out-dated-owncloud-directory-cache/
categories: howto
tags: howto linux bash owncloud directory cache
---

When copying user accounts and files from one dedicated [OwnCloud][oc] instance to another, I faced the problem of an out-dated directory cache. Some files could not be deleted and new files could not be created on the web interface. And the wordpress and apache logs strangely did not gave any indication of a problem. While not being a huge problem, but instead easily fixable, it still gave me some minutes to investigate.

<!--more-->

To reinitiate the OwnCloud directory cache for the user `USER`, there are some simple steps needed:

 - Look into the directory of the concerned `USER`. Ensure that all files and directory are given the correct ownership for the web server to access, e.g. `www-data:www-data`.

```
ls –lA /USERFILES/USER/files/some-directory/
```

- If the ownership is not correctly set, it should be fixed by the command

```
chown –R www-data:www-data /USERFILES/USER
```

- Once the ownership is assured, run the [OwnCloud command console][occ] to rescan the files of the `USER`.

```
chmod +x /var/www/owncloud/occ
sudo -u www-data /var/www/owncloud/occ files:scan USER
chmod -x /var/www/owncloud/occ
```

The process of rescanning might take some time, depending on the number of files on the `USER` profile. The occ should only be executable when needed, otherwise it could be abused by a rouge webserver. That's why its executable flag should be removed once you are finnished.



[occ]: https://doc.owncloud.org/server/9.0/admin_manual/configuration_server/occ_command.html
[oc]: https://owncloud.org/