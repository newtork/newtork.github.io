---
layout: post
title:  "New website stub: Simple Page"
date:   2016-12-01 18:00:00 +0000
modified: 2016-12-01 18:00:00 +0000 
comments: true
permalink: /weblog/2016/11/30/new-website-stub-simple-page/
categories: project website stub simplepage php html
excerpt_separator: <!--more-->
---

Taken from a little side project for a client who just wanted a simple website without any heavy requirements, I want to give you the little template engine I created and more importantly the static HTML approach I exercised: [Simple Page][simple]

<!--more-->

First of all, feel free to look into the [demo][demo].

### Features ###

 - static website for simple, multiple pages
 - no JavaScript, no reloading assets, no cookies, no dynamics
 - optimized style for all display sizes
 - custom form input styles

And when using PHP:

 - user inputs are stored into a local text file
 - security token when sending data to fight bots/spam
 - *i18n* - see the "/languages" directory

{% include tags/hint-start.html %}
If you are interested in the static page file, please just use the *save* feature of your browser.
{% include tags/hint-end.html %}

To keep things simple, I didn't implement nested templates or any logic, like iterations. That way the whole HTML template could fit into one file and the template engine stays rudimentary. Furthermore I wanted to avoid email interaction, for now. That's why logging to a text file should be a good enough start.

[simple]: https://github.com/newtork/website-stub/tree/master/simplepage
[stub]: https://github.com/newtork/website-stub
[demo]: http://newtork.de/simplepage/
