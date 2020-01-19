---
layout: post
title:  "How to Kill Inactive SSH Sessions"
date:   2020-01-18 09:00:00 +0300
categories: centos kill ssh gnu linux
comments: true
---

Sometimes you have a flaky internet connection and you may have been disconnected from your server a couple of times which resulted in inactive ssh sessions. When you look at details with [w](https://en.wikipedia.org/wiki/W_(Unix)) command, you see something like the following,

```txt
$ w
 15:22:29 up 77 days,  5:40,  5 users,  load average: 1.27, 0.88, 0.62
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     tty1                      03Nov19 77days  0.00s  0.00s -bash
onuryilm pts/0    10.10.90.1       14:51    3:49   0.02s  0.02s -bash
onuryilm pts/1    10.10.90.1       15:19    1:57   1.03s  1.03s htop
onuryilm pts/2    10.10.90.3       15:22   23.00s  0.00s  0.00s -bash
onuryilm pts/3    10.10.90.1       15:22    2.00s  0.00s  0.00s w
```

Here one of the records, which starts with "onuryilm", is active session but others are inactive SSH sessions.

By using [pstree](https://en.wikipedia.org/wiki/Pstree) commmand, you can find inactive sessions. It shows the running processes as a tree. As all the sessions are SSH connections, all related processes are expected to be grouped under `sshd` process.

```txt
$ pstree -p
systemd(1)─┬─NetworkManager(4926)─┬─{NetworkManager}(4972)
           │                      └─{NetworkManager}(4974)
...
           ├─sshd(5180)─┬─sshd(404)───sshd(406)───bash(407)───pstree(2009)
           │            ├─sshd(23390)───sshd(23392)───bash(23395)
           │            ├─sshd(31851)───sshd(31853)───bash(31855)───htop(32199)
           │            └─sshd(32640)───sshd(32646)───bash(32647)
...
```

---
**Note:** If your GNU/Linux distribution can't find `pstree` command, please install [psmisc](https://gitlab.com/psmisc/psmisc) package with your distribution's package manager. For CentOS users below command should be fine,

```bash
sudo yum install psmisc
```

---

Now look for the parent PID of the session – PID from the line without the `pstree` command. The `pstree` line, would be the active SSH session that you're using. To kill the idle sessions use [kill](https://en.wikipedia.org/wiki/Kill_(command)#Unix_and_Unix-like) command,

```txt
$ sudo kill 23390
$ sudo kill 31851
$ sudo kill 32640
```

Check afterwards with the [w](https://en.wikipedia.org/wiki/W_(Unix)) command,

```txt
$ w
 15:28:29 up 77 days,  5:46,  2 users,  load average: 0.71, 0.97, 0.76
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     tty1                      03Nov19 77days  0.00s  0.00s -bash
onuryilm pts/3    10.10.90.1       15:22    5.00s  0.01s  0.00s w
```

Happy coding.

---

{% include share-post-on-twitter.html %}
