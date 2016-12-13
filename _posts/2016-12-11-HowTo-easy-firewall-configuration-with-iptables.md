---
layout: post
title:  "HowTo: easy firewall configuration with iptables"
date:   2016-12-11 07:00:00 +0000
modified: 2016-12-11 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/11/howto-easy-firewall-configuration-with-iptables/
categories: howto
tags: linux bash firewall configuration iptables
---

Since Debian, the distribution I feel home at the most, unfortunately has weak capabilities to manage iptables by default, I found the following script to be very useful. It runs at startup, is easily editable and can be extended without much effort.

<!--more-->


 - Create a shell script `/etc/iptables_settings.sh`

```
#!/bin/sh
IPT="/sbin/iptables"

# Flush old rules, old custom tables
$IPT --flush
$IPT --delete-chain

# Set default policies for all three default chains
$IPT -P INPUT DROP
$IPT -P FORWARD DROP
$IPT -P OUTPUT ACCEPT

# Enable free use of loopback interfaces
$IPT -A INPUT -i lo -j ACCEPT
$IPT -A OUTPUT -o lo -j ACCEPT

# All TCP sessions should begin with SYN
$IPT -A INPUT -p tcp ! --syn -m state --state NEW -s 0.0.0.0/0 -j DROP

# Accept inbound TCP packets, Allow HTTP and HTTPS; SSH only from subnet
$IPT -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
$IPT -A INPUT -p tcp --dport 22 -m state --state NEW -s 10.0.1.0/24 -j ACCEPT
$IPT -A INPUT -p tcp --dport 80 -m state --state NEW -s 0.0.0.0/0 -j ACCEPT
$IPT -A INPUT -p tcp --dport 443 -m state --state NEW -s 0.0.0.0/0 -j ACCEPT

# Accept inbound ICMP messages
$IPT -A INPUT -p ICMP --icmp-type 8 -s 0.0.0.0/0 -j ACCEPT

# Accept outbound packets
$IPT -I OUTPUT 1 -m state --state RELATED,ESTABLISHED -j ACCEPT
```

 - Please mind the `--dport 22` line. It allows access to this port to a very limited local subnet mask. Feel free to either host the SSH daemon on a different port or have the port open to only a small range of valid IP addresses. It is **not** advised to have port **22** open for SSH while being completely open to the public **0.0.0.0/0**.
 - Additional ports can be set accordingly.

 
### Run the custom iptables configuration

If you are certain everything is setup correctly, and you are be able to access the machine even in case of a problem, *then* feel free to manually run your script.

```
/bin/sh /etc/iptables_settings.sh
```

If you are still connected, congratulation! Time to add the script to the system auto start.

 - Edit the file `/etc/rc.local` and add the following line **before** the last line `exit 0`.

```
[ -e '/etc/iptables_settings.sh' ] && /bin/sh /etc/iptables_settings.sh
```

 
