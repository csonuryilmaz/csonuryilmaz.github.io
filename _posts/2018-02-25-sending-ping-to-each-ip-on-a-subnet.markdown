---
layout: post
title:  "Sending Ping To Each IP on a Subnet - fPing"
date:   2018-02-25 00:00:00 +0300
categories: fping
comments: true
---

In order to check a PC is alive, we use **ping** command. But let's assume there is a PC in your network which is configured to get IP address automatically from DHCP server. On every boot, there is chance that it's IP address may be different from previous because of dynamic IP configuration. In order to find it in your subnet, you should **ping** all IPs to detect alive PCs. Then you can check those PC's hostnames or open ports to find the PC you want to reach.

**fping** is a good tool for this purpose. From its man page:
> **fping** differs from **ping** in that you can specify any number of targets on the command line, or specify a file containing the lists of targets to ping. Instead of sending to one target until it times out or replies, **fping** will send out a ping packet and move on to the next target in a round-robin fashion. In the default mode, if a target replies, it is noted and removed from the list of targets to check; if a target does not respond within a certain time limit and/or retry limit it is designated as unreachable.

If it's not available in system, first install with your package manager. For debian:
{% highlight bash %}
$ sudo apt install fping
{% endhighlight %}
After successfull installation, check it:
{% highlight bash %}
$ fping -v
fping: Version 3.15
fping: comments to david@schweikert.ch
{% endhighlight %}

You can learn your subnet with *inet address* and *netmask* by using **ifconfig** tool:
{% highlight bash %}
$ sudo ifconfig
...
wlp6s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.10  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::fd93:e37b:7eaf:e1d2  prefixlen 64  scopeid 0x20<link>
...
...
{% endhighlight %}
In our example, our subnet is 192.168.1.0 - 255. So we will try to ping 255 address to find out alive hosts. We will use the *mask* option:
{% highlight bash %}
$ fping -r 1 -g 192.168.1.0/24
192.168.1.1 is alive
192.168.1.10 is alive
192.168.1.7 is alive
192.168.1.2 is alive
...
192.168.1.177 is unreachable
192.168.1.178 is unreachable
192.168.1.179 is unreachable
192.168.1.180 is unreachable
192.168.1.181 is unreachable
...
{% endhighlight %}
If you don't define the *retry* `-r` limit default limit will be 3. This is the number of times an attempt at pinging a target will be made, not including the first try. *Generate* `-g` option will generate a target list of IPs from **netmask** or a **starting and ending IP**. For example;
{% highlight bash %}
$ fping -r 1 -g 192.168.1.1 192.168.1.10
192.168.1.1 is alive
192.168.1.7 is alive
192.168.1.10 is alive
192.168.1.2 is alive
192.168.1.3 is unreachable
192.168.1.4 is unreachable
192.168.1.5 is unreachable
192.168.1.6 is unreachable
192.168.1.8 is unreachable
192.168.1.9 is unreachable
{% endhighlight %}

Using the argument *alive* `-a` and *quiet* `-q` will restrict the output to reachable ip addresses. You may want to use it otherwise **fping** will also print unreachable addresses:
{% highlight bash %}
$ fping -aq -r 1 -g 192.168.1.0/24
192.168.1.1
192.168.1.2
192.168.1.10
192.168.1.7
{% endhighlight %}

You can print hostnames by using *name* `-n` argument. **fping** does a reverse-DNS lookup on them to get hostnames:
{% highlight bash %}
$ fping -aqn -r 1 -g 192.168.1.0/24
csp1.zte.com.cn
minibox
habion
android-6c61d22783ee0fff
{% endhighlight %}

But, in my opinion, both printing hostnames and IP addresses will better and more insightful. So, *name* `-n` argument in conjunction with *address* `-A` argument works like a charm:
{% highlight bash %}
$ fping -aq -An -r 1 -g 192.168.1.0/24
csp3.zte.com.cn (192.168.1.1)
minibox (192.168.1.7)
habion (192.168.1.10)
android-6c61d22783ee0fff (192.168.1.2)
{% endhighlight %}

#### More Information
* [fping homepage](https://fping.org)

Happy coding.

{% include share-post-on-twitter.html %}