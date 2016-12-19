---
title:  "HowTo: Fail2Ban and custom filters"
date:   2016-12-14 07:00:00 +0000
modified: 2016-12-14 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/14/howto-fail2ban-custom-filters/
categories: howto
tags: howto fail2ban apache security
---


The first time I encountered [Fail2Ban][f2b], and found it to be a useful tool, was when ransom malware has started to appear on client machines. Now there was a real risk the malware would propagate to the cloud storage platform. Fortunately the active malware *Locky* would place signature files and unique file extensions for encrypted files. With a tool like *Fail2Ban* we are able to monitor the webserver traffic for incriminating file uploads by the meta data, e.g. file names.


<!--more-->

*Fail2Ban* is a tool for banning IP addresses via *iptables*, given by lists of logical rules and filters on log files. Click on the blog *tag* above to find an introduction.


### Banning uploads by file names, e.g. *Locky* ransom malware

Add the following lines to the additional *Fail2Ban* configuration. It contains a week long ban time to quarantine the client machine.


{% assign shell-types = "root input" %}
{% include tags/shell-ind.html %}
```
vim /etc/fail2ban/jail.local

[apache-locky]
enabled  = true
port     = http,https
filter   = apache-locky
logpath  = /var/log/apache*/*access*.log
maxretry = 0
bantime  = 604800
```


Next, add the malware definition as *new* filter, create a file:


{% assign shell-types = "root input" %}
{% include tags/shell-ind.html %}
```
vim /etc/fail2ban/filter.d/apache-locky.conf

[Definition]
failregex = <HOST> \S+ \S+ \[.*?\] "PUT .*\.locky HTTP
            <HOST> \S+ \S+ \[.*?\] "PUT .*_Locky_recover_instructions\.txt
ignoreregex =
```

Any uploads of files ending with *\*.locky* or the *...instructions.txt* will be recognized and their owner will get banned.



 
### Banning malicous HTTP requests

Every minute a web server will be spammed with useless requests to test for server misconfigurations, e.g. proxy request. To block these malicious requests, I wrote a filter to block any abnormal request, not corresponding with a default request. **Note:** this is experimental.

{% assign shell-types = "root input" %}
{% include tags/shell-ind.html %}
```
vim /etc/fail2ban/jail.local

[apache-proxy]
enabled  = true
port     = http,https
filter   = apache-proxy
logpath  = /var/log/apache*/*access*.log
maxretry = 0
bantime  = 604800
```

As the example before, a new filter file needs to be created.

{% assign shell-types = "root input" %}
{% include tags/shell-ind.html %}
```
vim /etc/fail2ban/filter.d/apache-proxy.conf

[Definition]
# Matches lines such as:
# 192.168.1.1 - - "GET http://www.malicious.info/proxy.php ...

failregex = ^(?:(?![0-9\.]* \S+ \S+ \[.*?\] "([A-Z]* /.* HTTP/1\.[0-9]|-)")<HOST>)
ignoreregex =
``` 

[f2b]: http://www.fail2ban.org/
 