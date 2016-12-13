---
layout: post
title:  "New: website stub - Simple Page"
date:   2016-12-01 18:00:00 +0000
modified: 2016-12-01 18:00:00 +0000 
comments: true
permalink: /weblog/2016/12/01/new-website-stub-simple-page/
categories: project
tags: project css php html demo simplepage template
---

Taken from a little side project for a client who just wanted a simple website without any heavy requirements, I want to give you the little template engine I created and more importantly the static HTML approach I exercised: [Simple Page][simple]

<!--more-->

First of all, feel free to look into the [demo][demo].

![demo-image][simplepage0]

Often I find myself annoyed by a website using tons of scripts, animation, ads and external assets. Especially when having security concerns visiting a static site is quite pleasant surprise. That's why I try to stay away from JavaScript as much as I can. My approach consists of using CSS for instant page turns and PHP for the template and i18n. Cookies are disabled, but a session can be maintained by the *PHPSESSID* URL argument; although this is not recommended.

To keep things simple, I didn't implement nested templates or any logic, like iterations. That way the whole HTML template could fit into one file and the template engine stays rudimentary. Furthermore I wanted to avoid email interaction, for now. That's why logging to a text file should be a good enough start.

### Features

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



[simple]: https://github.com/newtork/website-stub/tree/master/simplepage
[stub]: https://github.com/newtork/website-stub
[demo]: http://newtork.de/simplepage/
[simplepage0]: /content-images/simplepage0.jpg