---
title:  "Snippet: Piping a process I/O with Bash"
date:   2016-12-04 07:00:00 +0000
modified: 2016-12-04 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/04/snippet-bash-piping-process/
categories: snippet
tags: howto bash snippet
---

While I worked on my [OpenTTD container][dockerottd], I came to the challenge of wrapping a process for initial and final input and output, and temporarily guarding it from interrupt signals. This is my solution.

<!--more-->

This is a template. Feel free to replace ``PROCESS``, ``"START"`` and ``"END"`` as you like.

{% gist 1fd583243a0b36d8aa3d139f95522723 %}



![diagram][dia]

[dockerottd]: https://github.com/newtork/docker-openttd
[dia]: /content-images/piping0.png