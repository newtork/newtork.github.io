---
layout: post
title:  "Docker: load-postgis"
date:   2016-12-06 07:00:00 +0000
modified: 2016-12-06 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/06/docker-load-postgis/
categories: project
tags: project docker bash postgres postgis database database
---

I'd like to introduce a little container I created, [load-postgis][load-postgis]. It loads PostgreSQL database backup files from a local directory and restores its content, with special respect to **g**eographic **i**nformation **s**ystems.

<!--more-->

The easiest way to manage data from [geographic information systems][gis] is to keep them in databases, specially designed to compute geometries, coordinates and graphs. A good choice is [PostGIS][postgis], a spatial database extension for PostgreSQL, which combined makes an outstanding system regarding performance, features and code quality.

Recently I started to revisit [an GIS university project][spatial] which I've worked on some years ago. To tidy up the development environment I've decided containerize the database. That way I'm no longer limited to my local machine and operation system, when working on the project.


## Usage

 - download container from [Docker Hub][dockerhub]
 
```
docker pull newtork/load-postgis
```

 - no database files to restore, port mapped to 5432
 
```
docker run -p 5432:5432 --rm newtork/load-postgis
```

 - one or more database files to restore from local directory
 
```
docker run -p 5432:5432 --rm -v /local/database/dumps/:/restore newtork/load-postgis
```

## Remarks

 - the container is only suitable for development purposes
 - accessible default login: *postgres/postgres*



{% include tags/hint-start.html %}
In case you still want to manually save the database changes, you may run...

`docker run -p 5432:5432 --rm --name psql -v /dumps/:/restore newtork/load-postgis`

`docker exec -it psql pg_dump -U postgres -Fc -f /restore/DB_NAME.BAK DB_NAME`
{% include tags/hint-end.html %}



[load-postgis]: https://github.com/newtork/docker-load-postgis
[dockerhub]: https://hub.docker.com/r/newtork/load-postgis/
[postgis]: http://postgis.net/
[gis]: https://en.wikipedia.org/wiki/Geographic_information_system
[spatial]: /weblog/2016/12/05/update-spatial-election