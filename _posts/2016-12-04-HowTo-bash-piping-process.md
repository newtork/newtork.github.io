---
layout: post
title:  "HowTo: Piping a process I/O with Bash"
date:   2016-12-04 07:00:00 +0000
modified: 2016-12-04 07:00:00 +0000 
comments: true
permalink: /weblog/2016/12/04/howto-bash-piping-process/
categories: howto
tags: howto pipe process io bash
---

While I worked on my [OpenTTD container][dockerottd], I came to the challenge of wrapping a process for initial and final input and output, and temporarily guarding it from interrupt signals. This is my solution.

<!--more-->

This is a template. Feel free to replace ``PROCESS``, ``"START"`` and ``"END"`` as you like.


```
#!/bin/bash

# start and run fifo pipeline with server
mkfifo myfifo
cat > myfifo &
pid_cat=$!
setsid PROCESS < myfifo &
pid_proc=$!

# get server status, 0=running, 1=terminated
running() {
	if ps -p $pid_proc > /dev/null
	then return 0
	else return 1
	fi
}

# trap function to execute commands after CTRL-C
savelyexit() {
	if running ; then
		echo "END" > myfifo
	fi

	kill $pid_cat &> /dev/null
	rm -f myfifo
	
	# sleep for short period of time to let trap quit smoothly
	sleep 1
	exit 0
}
trap savelyexit SIGINT SIGTERM EXIT
# additional signals could be INT SIGHUP

# send initial command to the server
echo "START" > myfifo

# check whether server has started and is still running
if running
then
	# read from stdin and redirect to fifo pipe while server is running
	while read line && running
	do echo "$line" > myfifo
	done < /dev/stdin
fi

exit 0
```



![diagram][dia]

[dockerottd]: https://github.com/newtork/docker-openttd
[dia]: /content-images/piping0.png