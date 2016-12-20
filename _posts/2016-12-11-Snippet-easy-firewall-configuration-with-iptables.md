---
title:  "Snippet: easy firewall configuration with iptables"
date:   2016-12-11 07:00:00 +0000
modified: 2016-12-11 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/11/snippet-easy-firewall-configuration-with-iptables/
categories: snippet
tags: linux bash firewall iptables snippet
---

Since Debian, the distribution I feel home at the most, unfortunately has weak capabilities to manage iptables by default, I found the following script to be very useful. It runs at startup, is easily editable and can be extended without much effort.

<!--more-->


 - Create a shell script `/etc/iptables_settings.sh`

{% gist 9c4649a0ebaeeb65ba5a49df55496365 %}



 - Please mind the `--dport 22` line. It allows access to this port to a very limited local subnet mask. Feel free to either host the SSH daemon on a different port or have the port open to only a small range of valid IP addresses. It is **not** advised to have port **22** open for SSH while being completely open to the public **0.0.0.0/0**.
 - Additional ports can be set accordingly.

 
### Run the custom iptables configuration

If you are certain everything is setup correctly, and you are be able to access the machine even in case of a problem, *then* feel free to manually run your script.

{% include tags/shell-ind.html types="root" %}
```
/bin/sh /etc/iptables_settings.sh
```

If you are still connected, congratulation! Time to add the script to the system auto start.

 - Edit the file `/etc/rc.local` and add the following line **before** the last line `exit 0`.

{% include tags/shell-ind.html types="root input" %}
```
vim /etc/rc.local
[ -e '/etc/iptables_settings.sh' ] && /bin/sh /etc/iptables_settings.sh
```

 
