---
title:  "New: Website - Photography - pictrace.de"
date:   2025-11-23 07:00:00 +0000
modified: 2025-11-23 07:00:00 +0000 
comments: true
permalink: /weblog/2025/11/23/new-website-photography/
categories: project 
tags: javascript css html project photography
---

Earlier this year I decided to commit further to my photography hobby.
My intention was to observe, document, and present my progress in technique and creativity.
Instead of relying on and opening up to 3rd party platforms, I chose to start my own little website for that.
Especially the format of presenting images primarily, without text and animation - it's an interesting challenge in web design and choice of technology to apply.

Please find my material here: [pictrace.de](https://pictrace.de)

<!--more-->

Key requirements I set for myself:
* Single HTML
* Hosted on GitHub Pages via Jekyll
* JavaScript must be optional
* Animation, transition and navigation must be handled via CSS
* Avoid unnecessary complexity
* Avoid unnecessary dependencies

Resolved difficulties:
* **(Major)** Adaptive view depending on screen size.
* **(Major)** I migrated from `float` to `flex-box` to `grid` view. Each with amazing improvements. Over the years we did come a long way from `table`.
* **(Major)** Lazy loading images greatly improves performance. This is a huge game-changer for big galleries.
* **(Minor)** Contact form via Google form and -automation. This is a nice approach to avoid third parties other than Google. I may take this as future inspiration for comment sections.

Unresolved pain-points:
* **(Major)** Display is rather dark on mobile devices. I have no idea how to improve that.
* **(Minor)** HTML is not compressed. I think I can improve load times further.
* **(Minor)** I could research a little on fullscreen toggles. Maybe WebGL or similar technology could assist.


[![pictrace.png](/content-images/2025-11-23-pictrace.png)](https://pictrace.de)


Please find my material here: [pictrace.de](https://pictrace.de)