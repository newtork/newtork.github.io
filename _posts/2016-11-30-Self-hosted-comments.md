---
layout: post
title:  "Self-hosted comments"
date:   2016-11-30 08:00:00 +0000
modified: 2016-11-30 08:00:00 +0000 
comments: true
permalink: /weblog/2016/11/30/self-hosted-comments/
categories: meta blog server
excerpt_separator: <!--more-->
---

Communication on a blog doesn't inevitably has to be unidirectional. More than once I found solutions to my tech problems in the comments section of a related thread, thanks to popular search engines. Having a static website, usually means embedding a third-party commenting service, most often [Disqus][disqus]. There are alternatives!

<!--more-->

### Disqus ###

[Disqus][disqus] is a feature rich comment system service. When not talking about WordPress, it is probably the most popular one. Unfortunately distributed by a commercial American company, it is neither open source nor self hosting. And when talking about user privacy and their data protection, these are some serious downsides, which have to be considered.

Other competitors in the same segment are [muut][muut], [Adobe Livefyre][livefyre] and [Jetpack for WordPress][jetpack]


### Isso ###

Isso, an acronym on the old German saying: *Ich schrei' sonst* / *Otherwise, I scream.*. It is used to establish comments on websites otherwise being without any interactivity. On a dedicated, self-hosted web server you can manage all user comments by yourself.

 - **Python** is required. The data is saved to **SQLite**.
 - Easy installation, though with pitfalls.
 - Easy **JavaScript** integration, clear generated UI.
 - Allows parallel usage on multiple domains.
 - Automatically determines UI language.
 - Other decent features: voting mechanism, sub-threading, edit/delete for a short period after posting, admin notification via SMTP, whitelisted HTML tags.

 
Unfortunately *isso* is not logging posts and user interactions to its logfile and some of its incomprehensible actions during setup and runtime are "expected behaviour". Even editing or deleting older posts means trouble.

{% include tags/hint-start.html %}
To the date of my installation of *isso* **0.10.6**, it was *not* possible to declare custom thread ids, like an incrementing identifier. Instead, *isso* uses the URL path of the thread. It has to be publicly accessible by **[HOST] + URI**. If *isso* is unable to reach the public URL, it will not carry on.
{% include tags/hint-end.html %}

{% include tags/hint-start.html %}
To the date of my installation of *isso* **0.10.6**, *isso* will return HTTP **404** when looking for posts in a thread and none has been made yet.
{% include tags/hint-end.html %}

{% include tags/hint-start.html %}
To the date of my installation of *isso* **0.10.6**, *isso* will not be able to log to *stdout*.
{% include tags/hint-end.html %}

As my website is running on [Apache][https://httpd.apache.org/], I used *VirtualHost* configurations like the ones below:

```
<VirtualHost *:80>
        ServerName comments.newtork.de
        Header set Access-Control-Allow-Origin "http://blog.newtork.de"

        ProxyPreserveHost Off
        ProxyRequests Off
        ProxyPass        "/" "http://0.0.0.0:8095/"
        ProxyPassReverse "/" "http://0.0.0.0:8095/"

        ErrorLog ${APACHE_LOG_DIR}/newtork.de/comments/error.log
        CustomLog ${APACHE_LOG_DIR}/newtork.de/comments/access.log combined
</VirtualHost>
<VirtualHost *:443>
        ServerName comments.newtork.de
        Header set Access-Control-Allow-Origin "http://blog.newtork.de"

        ProxyPreserveHost Off
        ProxyRequests Off
        ProxyPass        "/" "http://0.0.0.0:8095/"
        ProxyPassReverse "/" "http://0.0.0.0:8095/"

        SSLEngine on
        SSLCertificateFile /MY/LETSENCRYPT/CERTIFICATE.crt
        SSLCertificateKeyFile /MY/LETSENCRYPT/KEYFILE.key
        SSLCertificateChainFile /THE/LETSENCRYPT/CHAINFILE.crt

        ErrorLog ${APACHE_LOG_DIR}/newtork.de/comments/error-ssl.log
        CustomLog ${APACHE_LOG_DIR}/newtork.de/comments/access-ssl.log combined
</VirtualHost>
```

By default, accessing the comments is happening with SSL, obviously for privacy reasons.

[disqus]: https://disqus.com/
[muut]: https://muut.com
[livefyre]: http://www.adobe.com/marketing-cloud/enterprise-content-management/ugc-content-platform.html
[jetpack]: https://jetpack.com/