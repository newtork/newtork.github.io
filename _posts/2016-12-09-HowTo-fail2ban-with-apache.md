---
layout: post
title:  "HowTo: Use fail2ban with apache for security"
date:   2016-12-09 07:00:00 +0000
modified: 2016-12-09 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/09/howto-fail2ban-with-apache/
categories: howto
tags: howto fail2ban apache security ban ip
---


[Fail2Ban][f2b] is a great tool for linux to monitor log files of various programs and look for malicious attempts from attackers. Once an illicit request or action is registered or it exceeded a threshold in number, the IP address will get banned for a defined period of time, making it harder for an attacker to continue the system penetration. This short tutorial will give you a quick introduction for setting it up.

<!--more-->

*Fail2Ban* is a tool for banning IP addresses via *iptables*, given by lists of logical rules and filters on log files. The service behaviour is defined by one (*jail.conf*) or more (*\*.local*) configuration files, which control the settings of so called "jails".

The jails will define

 - a status indicator to *enable* or disable the jail
 - a *filter* to match malicious behaviour in text
 - a *log path* to define the text file to read from
 - *ban duration* in seconds
 - allowed number of *retries*
 - a list of *ports* the IP would be banned from
 

## Installation

The easiest way to install *Fail2Ban* is by using the repository.

```
apt-get update
apt-get install fail2ban
touch /etc/fail2ban/jail.local
```

 - `/etc/fail2ban/jail.conf` gives the **default** configuration.
 - `/etc/fail2ban/jail.local` gives **additional** configuration.
 
 
## Configuration

 - It is possible to take and copy any setting from the *jail.conf* file to the **jail.local** file and change the value, like enabling or disabling filters and changing *retry* counters and *bantime* durations.

 - The following filters are enabled / disabled by default:

| Name | Status | Notes |
|---|---|---|
| ssh | ☑ | Activate to block unusual secure shell logins requests |
| dropbear | ☐ |  |
| pam-generic | ☐ |  |
| xinetd-fail | ☐ |  |
| ssh-ddos | ☐ |  |
| ssh-route | ☐ |  |
| ssh-iptables-ipset4 | ☐ |  |
| ssh-iptables-ipset6 | ☐ |  |
| apache | ☐ | Activate to block failed web authentication requests |
| apache-multiport | ☐ | Deprecated |
| apache-noscript | ☐ | Activate to block failed web requests for scripts, e.g. cgi, pl, asp |
| apache-overflows | ☐ | Activate to block web requests on a long or suspicious nature, e.g. URL overflows |
| apache-badbots | ☐ | Activate to block known spambots and software alike, by checking the HTTP user agent |
| apache-nohome | ☐ | Activate to block failed web requests for home directories |
| apache-modsecurity | ☐ |  |
| php-url-fopen | ☐ | Experimental |
| lighttpd-fastcgi | ☐ |  |
| lighttpd-auth | ☐ |  |
| nginx-http-auth | ☐ |  |
| roundcube-auth | ☐ |  |
| sogo-auth | ☐ |  |
| vsftpd | ☐ |  |
| proftpd | ☐ |  |
| pure-ftpd | ☐ |  |
| wuftpd | ☐ |  |
| postfix | ☐ |  |
| couriersmtp | ☐ |  |
| courierauth | ☐ |  |
| sasl | ☐ |  |
| dovecot | ☐ |  |
| mysqld-auth | ☐ |  |
| named-refused-udp | ☐ | Prohibited |
| named-refused-tcp | ☐ |  |
| freeswitch | ☐ |  |
| ejabberd-auth | ☐ |  |
| asterisk-tcp | ☐ |  |
| asterisk-udp | ☐ |  |
| recidive | ☐ |  |
| ssh-blocklist | ☐ |  |
| nagios | ☐ |  |



 - For some programs, additional configuration will be required, to log failed login attempts and other illegal operations. Here, that'll be *mysqld-auth*:

```
vim /etc/my.cnf
+ log-error=/var/log/mysqld.log
+ log-warning = 2
```

 - But let's first activate the mail notification feature. The default action shall be a ban and mail notification with whois-information and the log lines being detected.

```
vim /etc/fail2ban/jail.local

[DEFAULT]
destemail     = youraccount@email.com
sender        = fail2ban@localhost
sendername    = Fail2Ban Alerts
mta           = mail
action        = %(action_mwl)s
```



 - Now activate the apache filters above. This shell command iterates over a list of filters (*apache\**) and inserts them as jail with same settings to the *additional* configuration. Feel free to change the list of filters and the apache logpath provided.

```
for filter in apache apache-noscript apache-overflows apache-badbots apache-nohome; do printf "\
\
[$filter]\n\
enabled  = true\n\
filter   = $filter\n\
logpath  = /var/log/apache*/*error*.log\n\
maxretry = 1\n\
\
" $filter >> /etc/fail2ban/jail.local; done
```


 - You can reload the *Fail2Ban* configuration with the usual *service* command.

```
service fail2ban reload
-or-
/etc/init.d/fail2ban reload
```



{% include tags/hint-start.html %}
To customize the mail notification settings, have a look at `/etc/fail2ban/action.d/mail-whois-lines.conf`. Here you could comment out `actionstart` and `actionstop` when the mail notification pile up. Or you could add a notification for `actionunban` or an additional `actioncheck` command before a ban happens.
{% include tags/hint-end.html %}

 


{% include tags/hint-start.html %}
*Fail2Ban* works with iptables. You can observe changes and blocked IPs via `iptables -L`
{% include tags/hint-end.html %}
 
 


{% include tags/hint-start.html %}
If *Fail2Ban* behaves different than expected, you can debug it by `fail2ban-client -x start`
{% include tags/hint-end.html %}

 


{% include tags/hint-start.html %}
To set up an email configuration for your server, you can use *exim4*. Please find a suitable tutorial online.

```
apt-get install exim4

dpkg-reconfigure exim4-config
```

If you encounter difficulties, you should take a look into the *exim4* log file:

```
less /var/log/exim4/mainlog

tail -f /var/log/exim4/mainlog
```

Don't forget configuring the firewall for required open ports.

{% include tags/hint-end.html %}




## Revoking a ban

To take back an already performed ban, the following type of command should be executed with *elevated privileges*:
 
``` 
fail2ban-client get [FILTER] actionunban [IP]
fail2ban-client get apache-owncloud actionunban 123.145.167.189
```

To stop additional bans while working on a problem, stopping the service is advised:
 
```
service fail2ban stop
```

 
 

[f2b]: http://www.fail2ban.org/