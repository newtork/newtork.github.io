---
title:  "Fork: Paper Page - Paper Flow (WordPress Theme)"
date:   2016-12-10 07:00:00 +0000
modified: 2016-12-10 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/10/Fork-paper-page-paper-flow/
categories: project
tags: website wordpress php html paperflow
---

In order to give the [Paper Page][paperpage] website template a little spin, I decided to implement it as a [WordPress theme][wordpressthheme]. That way the content and template management will be easily achieved. And here it is: [Paper Flow][paperflow].

<!--more-->

First of all, feel free to look at the [showcase][deafflow].


![demo-image0][paperflow-showcase0] 
![demo-image1][paperflow-showcase1]


### Features
 
 - **custom logo, custom background**
 - **custom featured image placement and dimension**
 - **custom post box ordering, placement and dimension**
 - **categories as bookmarks**
 - JavaScript driven page turning
 - optimized for touch devices and displays of all sizes
 - fall back stylesheet for a display too small
 - custom static rotation of booklet


### Requirements

 - Functioning WordPress installation
 - [Meta Box][metabox] WordPress plugin


### Usage

1. Download the [theme files][paperflow] from GitHub to your local `/wordpress/wp-content/themes/`*paperflow* path and activate it.
2. Create a WordPress category `paperflow` and add sub categories with descriptions e.g. `crew`, `cargo`, `cruise`. They will appear as booklet bookmarks.
3. Move or create WordPress posts to the sub categories. When editing a post, take note of the meta fields at the bottom. Here you can change placement and dimension of the post and featured image.


{% include tags/hint-start.html %}
Box dimensions of *posts* in a *category* should sum up to 100% in width and height.
{% include tags/hint-end.html %}




### Limitations

Unfortunately when using *paperflow* as WordPress theme, you are not given the full functionality you'd expect from popular themes.

 - As of version 0.1 this WordPress theme only supports displaying the start page.
 - No single posts, no dedicated pages, no comments, nor preview are supported so far.

 
 
But I'm going to add the missing WordPress theme features soon enough, as you will see in the [GitHub repository][paperflow].




[paperpage]: https://github.com/newtork/website-stub/tree/master/paperpage
[stub]: https://github.com/newtork/website-stub
[wordpressthheme]: https://wordpress.org/themes/
[deafflow]: https://deafflow.com/
[paperflow]: https://github.com/newtork/website-stub/tree/master/paperflow
[metabox]: https://wordpress.org/plugins/meta-box/
[paperflow-showcase0]: /content-images/paperflow-showcase0.jpg
[paperflow-showcase1]: /content-images/paperflow-showcase1.jpg
