---
layout: post
title:  "Old: spatial.election will return in 2017"
date:   2016-12-01 07:00:00 +0000
modified: 2016-12-01 07:00:00 +0000 
comments: true
permalink: /weblog/2016/11/30/old-spatial-election-will-return/
categories: project website java spacial
---

In less than a year, there will be the next [elections for the Bundestag in Germany][election]. Again we will face interesting political outcomes and changing trends on the level of country, state and city, even district. Thanks to the wonders of the World Wide Web, we can acknowledge statistics to almost anything in high definition. In a little web project I did some years ago, I examined political, social and economical data of districts in Germany with regard to spatial properties and the results of the federal election in 2013: [spatial.election][spatial]

<!--more-->

First of all, here is the [demo][demo].

I have used the opportunity to upload my project again to a live environment. Even though it has been years since I touched it, I will be more than happy to upload new data about the same time next year. With a little luck we will see additional trends in how much change has happened in a spatial perception.

Unfortunately it is currently not possible to run the *WAR* file on the latest [Tomcat][tomcat] server. But thanks to the jetty runtime environment, the newly compiled, portable version is still good to go. Hopefully I'm going to find a little time during the upcoming months to refurbish the project: the visuals, load time and resource consumption is subpar.

### Documentation ###

The last time I touched the project, was to compose a [Wiki documentation][wiki].

### Details ###

 - Java project, made with Eclipse
 - PostgreSQL database with PostGIS extension
 
 
### Extended database installation instructions ###

1. Download and install dependencies.

```
apt-get update
apt-get install postgis
```

2. Create database "spatial_election" with extensions "postgis" and "postgis_topology".

```
createdb spatial_election --username=postgres --encoding=UNICODE
psql spatial_election -U postgres -c "CREATE EXTENSION postgis; CREATE EXTENSION postgis_topology;"
```

3. Set passsword for postgres.

```
postgres:~$ psql -U postgres -c "\password"
Enter new password: postgres
Enter it again: postgres
```

4. Allow login via password, *md5* instead of *peer*.

```
sed 's/^local\s*all\s*postgres\s*peer/local\tall\tpostgres\tmd5/' /etc/postgresql/9.*/main/pg_hba.conf
service postgresql restart
```

5. Restore database from the dump file, here *spatial_db.backup*.

```
pg_restore -U postgres -d spatial_election spatial_db.backup
Password: postgres
```
{% include tags/hint-start.html %}
You may encounter an error message during the restore procedure. You can ignore it:
```
[...]
pg_restore: [archiver (db)] could not execute query: ERROR:  schema "topology" already exists
Command was: CREATE SCHEMA topology;
WARNING: errors ignored on restore: 1
```
{% include tags/hint-end.html %}


{% include tags/hint-start.html %}
Close *psql* sessions with **\q**
{% include tags/hint-end.html %}




[election]: https://en.wikipedia.org/wiki/Electoral_system_of_Germany
[spatial]: https://github.com/a-d/spatial.election/
[demo]: https://newtork.de/spatial.election/
[wiki]: https://github.com/a-d/spatial.election/wiki