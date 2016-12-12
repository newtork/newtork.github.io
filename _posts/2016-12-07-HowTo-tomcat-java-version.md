---
layout: post
title:  "HowTo: Change the Java Runtime Environment of Tomcat"
date:   2016-12-07 07:00:00 +0000
modified: 2016-12-07 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/07/howto-tomcat-java-version/
categories: howto bash tomcat java
---

When working with Debian the default repositories often only provide software versions of older age. That's why as of today the Tomcat server will be only distributed with Java 7. And if you are required to use a more recent version of this programming language, you'll need to tell Tomcat which binary to use.

<!--more-->

For a recently upgraded Java project ([spatial election][spatial.election]) I needed [Tomcat][tomcat] to run a newer version of the runtime environment and development kit.

### Java

 - First of all, let's check the currently installed version of Java in the execution path.

```
java -version
javac -version
```

 - If it is not giving the desired version, check the available binaries.

```
ls -dl /usr/lib/jvm/java-*
```

 - If this is not listing the desired version to run, a manual download and installation of the required binary is needed, e.g. Oracle Java 8 for Debian/Ubuntu.


```
echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" > /etc/apt/sources.list.d/java-8-debian.list
echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" >> /etc/apt/sources.list.d/java-8-debian.list
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EEA14886
apt-get update
apt-get install oracle-java8-installer
```

	Usually the repository packages hosted on the [Canonical launchpad platform][launchpad] would not support Debian hosts, because they are built against Ubuntu libraries. But this package distributed by the [WebUpd8][webupd8] team only contains an installer, making it conveniently work on Debian too.


### Tomcat

 - Check the active Java runtime environment on the Tomcat server manager status page: `http://localhost:8080/manager/status`

[!before][img-tom-old]
([zoom][img-tom-old-big])

 - If the displayed version does not suffice, the following changes are needed in `/etc/default/tomcat7` to set a different binary path.

```
vim /etc/default/tomcat7
- #JAVA_HOME=/usr/lib/jvm/openjdk-6-jdk
+ JAVA_HOME=/usr/lib/jvm/java-8-oracle

service tomcat7 restart
```

 - A follow up check on the Tomcat server manager status page should yield the correct JVM version,

[!after][img-tom-new]
([zoom][img-tom-new-big])



 
[launchpad]: https://launchpad.net/
[webupd8]: http://www.webupd8.org/
[spatial.election]: 
[tomcat]: https://packages.debian.org/jessie/tomcat7
[img-tom-new]: /content-images/tomcat_jvm_new_small.jpg
[img-tom-new-big]: /content-images/tomcat_jvm_new.jpg
[img-tom-old]: /content-images/tomcat_jvm_old_small.jpg
[img-tom-old-big]: /content-images/tomcat_jvm_old.jpg