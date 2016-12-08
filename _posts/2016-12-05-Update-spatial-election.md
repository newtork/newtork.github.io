---
layout: post
title:  "Update: spatial.election 2.0"
date:   2016-12-05 07:00:00 +0000
modified: 2016-12-05 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/05/update-spatial-election/
categories: project website java spatial
---

When I [revisited][metaspatial] my old [spatial.election][spatial] uni project, I faced several issues while trying to start it on modern versions of Java, PostgreSQL and Tomcat. I seized the opportunity to give it a major update and fix some old bugs.


<!--more-->

First of all, here is the [demo][demo].

![v1small][imgV1s] ![v2small][imgV2s]

Preview Images | [V1.0][imgV1] | [V2.0][imgV2]


## Downloads

- Tomcat WAR | [1.0][w1] | [2.0][w2]
- Executable JAR | [1.0][j1] | [2.0][j2]
- PSQL Database | [1.0][db1] | [2.0][db2]


### Requirements for spatial.election 2.0

 - PostgreSQL ~~9.3~~ 9.6
 - PostGIS ~~2.1~~ 2.3.1
 - Tomcat *7.0*
 - Java ~~1.7~~ 1.8

### Run

The easiest way to run the software, is to run an up-to-date *Java* and my Docker [Postgis container][docker]. Here are the details...

#### Java

 - Check your current Java Runtime Environment, it should be at least version *1.8*.

```
java -version

java version "1.8.0_111"
Java(TM) SE Runtime Environment (build 1.8.0_111-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.111-b14, mixed mode
```

#### Database

The database adapter expects a connectable PostgreSQL database *spatial_election* on local port *5432*. There should be a user *postgres* with password *postgres*. You could change any of these settings by editing the source XML or by altering the compiled ``(jar|war)\WEB-INF\lib\database-2.0.jar\META-INF\persistence.xml``.

 - **(a)** Check your current PostgreSQL installation, it should be at least version *9.6*. Make sure PostGIS is installed as well.

```
psql --version
dpkg -l | grep postgis
```

 - **(b)** Just use my container [docker-load-postgis][docker] to run the GIS database. 

#### Execution

 - **(a)** Install and run *Tomcat*. Upload and deploy the **WAR** file provided. If the startup fails, check the [Java Version which the Tomcat Server runs on][metatomcat].
 - **(b)** Simply run the executable **JAR** file. *Note:* the port number might change.

```
java -jar server##2.0-20161203.jar
```


### Changes

 - [*general*] new dependencies with new methods, e.g. jetty moved from *org.codehaus* to *org.eclipse*
 - [*general*] moved from direct Hibernate implementation to JPA API
 - [*database*] fixed key column types on various tables: *smallint, bigint, integer* to *integer*
 - [*website*] changed visuals from insignificant "Erststimme" to important "Zweitstimme", fixed bug in calculating colour intensity.


[spatial]: https://github.com/a-d/spatial.election/
[demo]: https://newtork.de/spatial.election/
[w1]: https://newtork.de/dl/spatial_election/spatial.election##1.0-0.1.war
[w2]: https://newtork.de/dl/spatial_election/spatial.election##2.0-0.1.war
[j1]: https://newtork.de/dl/spatial_election/server##1.0-20161202.jar
[j2]: https://newtork.de/dl/spatial_election/server##2.0-20161203.jar
[db1]: https://newtork.de/dl/spatial_election/spatial_election.bak1
[db2]: https://newtork.de/dl/spatial_election/spatial_election.bak2
[docker]: https://github.com/newtork/docker-load-postgis
[metatomcat]: /weblog/2016/12/07/howto-tomcat-java-version
[metaspatial]: /weblog/2016/12/02/old-spatial-election-will-return
[imgV1]: https://newtork.de/dl/spatial_election/preview01.png
[imgV2]: https://newtork.de/dl/spatial_election/preview02.png
[imgV1s]: https://newtork.de/dl/spatial_election/preview01_small.png
[imgV2s]: https://newtork.de/dl/spatial_election/preview02_small.png