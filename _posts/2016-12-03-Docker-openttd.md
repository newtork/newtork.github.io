---
layout: post
title:  "Docker: openttd"
date:   2016-12-03 07:00:00 +0000
modified: 2016-12-03 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/03/docker-openttd/
categories: project docker bash openttd
---

My first steps with docker have been quite exciting. I decided to create a container to easily run an [OpenTTD][dockerottd] gameserver. This build comes with some neat little features, to stick out from conventional OpenTTD containers.

<!--more-->

## Features

 - During **build** the container automatically downloads the latest stable version of [OpenTTD][openttd] and additionally the required asset files.
   - Files which have been downloaded are being verified by their checksum.
   - A default gameserver configuration file can be attached to the build.
   
 - During **run** the containers's shell script tunnels the gameservers I/O.
   - You can use the gameserver commands, like saving, loading, announcements, pausing, restarting, ...
   - You can set a save-game name and additional runtime configurations when starting the container, to load automatically.
   - When terminating the container the game will be saved.

   
## Usage
```
newtork/openttd help
newtork/openttd [save [config [name]]]
```



## Explanation

```
	docker run -dit -p 3979:3979/tcp -p 3979:3979/udp newtork/openttd [savegame] [settings] [servername]

	\_____________/ \_______________________________/ \_____________/  \______/  \________/  \_________/
          |                        |                      |              |           |            |
     interactive       required port forwarding      maintainer     save game   config string   public game
      detached          destination port can be        build          name      \n-seperated    server name
      terminal           changed as required                           
```

# Example
```
	docker run -dit -p 3979:3979/tcp -p 3979:3979/udp newtork/openttd
	docker run -dit -p 3979:3979/tcp -p 3979:3979/udp newtork/openttd save1
	docker run -dit -p 3979:3979/tcp -p 3979:3979/udp newtork/openttd save2 "$(</local/openttd.cfg)"
	docker run -dit -p 3979:3979/tcp -p 3979:3979/udp newtork/openttd save3 "map_x=4 \n map_y=5" "My Favorit Server"
	docker run -dit -p 3979:3979/tcp -p 3979:3979/udp -v /local/saves/:/root/.openttd/save/ newtork/openttd save4 "map_x=4 \n map_y=5" "My Favorit Server"

```

	


[dockerottd]: https://github.com/newtork/docker-openttd
[openttd]: http://www.openttd.org/en/