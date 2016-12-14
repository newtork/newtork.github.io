---
title:  "Meta: Run local Jekyll blog with docker"
date:   2016-12-16 08:00:00 +0000
modified: 2016-12-16 08:00:00 +0000 
comments: true
permalink: /weblog/2016/12/16/jekyll-docker/
categories: blog
tags: blog meta jekyll docker
---

Since I often find myself working on a Windows machine, it would be quite troublesome to run a dedicated ruby environment for Jekyll and its dependencies. That's why when editing my blog I'm happy to use the official [Jekyll][jekylldocker] docker build.

<!--more-->

Pull the container:

```
docker pull jekyll/jekyll:pages
```

Run it with a mount to your local blog:

```
docker run --rm --volume=/local/blog:/srv/jekyll -it -p 4000:4000 jekyll/jekyll:pages
```

Run it with automatic file updates:

```
docker run --rm --volume=/local/blog:/srv/jekyll -it -p 4000:4000 jekyll/jekyll:pages jekyll serve --incremental --watch --force_polling
```

**Note:** transitive changes, e.g. *posts by tag* or *posts by category*, are not affected.

[jekylldocker]: https://hub.docker.com/r/jekyll/jekyll/