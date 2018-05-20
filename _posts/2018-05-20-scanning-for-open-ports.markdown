---
layout: post
title:  "Scanning For Open Ports"
date:   2018-05-20 00:00:00 +0300
categories: port netstat
comments: true
---

A port is **open** when there's a process running on the machine which is listening on that port for incoming requests. For example, default web server open port is HTTP (80) and ssh is SSH (20).

Using **netstat** we can list open ports and active processes that listen them:
```
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:5941          0.0.0.0:*               LISTEN      565/teamviewerd     
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      466/sshd            
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      9208/cupsd          
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      821/exim4           
tcp6       0      0 :::22                   :::*                    LISTEN      466/sshd            
tcp6       0      0 ::1:631                 :::*                    LISTEN      9208/cupsd          
tcp6       0      0 ::1:25                  :::*                    LISTEN      821/exim4
...
```
* `-n` Include the numeric ports and addresses.
* `-l` Include only listening sockets.
* `-p` Add the process ID (PID) of the listening process/program.

Happy coding.

{% include share-post-on-twitter.html %}