---
layout: post
title:  "Connect To Workstation With Hamachi"
date:   2018-02-04 00:00:00 +0300
categories: hamachi
comments: true
---

Let's assume you have a workstation (or a pc) at office. For some various reasons, you don't have VPN ability to your corporate network. But you want to have remote access to your office workstation, by this way you can work remotely. This was my case and I installed a LAN over *internet* with **hamachi**, I added office workstation and home laptop to the same network, I connected to my office workstation with **ssh** and did all my job. So this is how I did it!

Office workstation is a 64 bit **debian 8.10** desktop. Home laptop is a 32 bit **debian 9.3** [Here][hamachi download page] **Hamachi** download page for GNU/Linux. You can find both x86/x64 packages, download and install.

x86
{% highlight bash %}
$ wget https://www.vpn.net/installers/logmein-hamachi_2.1.0.174-1_i386.deb

$ sudo gdebi logmein-hamachi_2.1.0.174-1_i386.deb
{% endhighlight %}

x64
{% highlight bash %}
$ wget https://www.vpn.net/installers/logmein-hamachi_2.1.0.174-1_amd64.deb

$ sudo gdebi logmein-hamachi_2.1.0.174-1_amd64.deb
{% endhighlight %}

**Note:** Alternatively, you can use `sudo dpkg -i` in order to install **deb** packages. Also [hamachi documentation][logmein hamachi user guide] contains *How to Install the Client to a Local Computer* section with some details.

After installation you can check version:
{% highlight bash %}
$ sudo hamachi --version
{% endhighlight %}
If everything is ok, you will see version with an overview of actions.

On host computer (in my case *office workstation*) log into your LogMeIn account and go online:
{% highlight bash %}
$ sudo hamachi login
{% endhighlight %}

Display the details and the current status of the client:
{% highlight bash %}
$ sudo hamachi
  version    : 2.1.0.174
  pid        : 26771
  status     : logged in
  client id  : 221-094-578
  address    : 25.66.125.69    2620:9b::1942:7d45
  nickname   : onian
  lmi account: -
{% endhighlight %}

Create a network on host computer:
{% highlight bash %}
$ sudo hamachi create onianofficelan
Password: 
Creating onianofficelan .. ok
{% endhighlight %}
*onianofficelan* is **unique id** for your network. You can define any unique id to your network. Also you create a password for network. In order to join your network, other client must use this password with network name.

List available networks and clients connected to them:
{% highlight bash %}
$ sudo hamachi list
 * [onianofficelan]  capacity: 1/5, subscription type: Free, owner: This computer
{% endhighlight %}
As you see, our subscription is free and it supports up to 5 peers in free mode.

On remote computer (in my case *home laptop*) log into your LogMeIn account and go online as the same as host computer: `$ sudo hamachi login` Then join to network which is created by host computer:
{% highlight bash %}
$ sudo hamachi join onianofficelan
[sudo] password for onuryilmaz: 
Password: 
Joining onianofficelan .. ok
{% endhighlight %}

After join, the status of network is:
{% highlight bash %}
$ sudo hamachi list
 * [onianofficelan]  capacity: 2/5, subscription type: Free, owner: onian (221-094-578)
     * 221-094-578   onian                      25.66.125.69      alias: not set           2620:9b::1942:7d45                          direct      UDP  85.106.8.58:42806
{% endhighlight %}

Now, we're in the same network with office workstation. If you have configured SSH server with `X11Forwarding yes` option from **/etc/ssh/sshd_config**, you can connect to remote computer by **ssh** with `-X` option:
{% highlight bash %}
$ ssh -X onuryilmaz@25.66.125.69
onuryilmaz@25.66.125.69's password: 

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
You have new mail.
Last login: Sun Feb  4 17:28:50 2018 from 25.65.75.187
{% endhighlight %}

With `-X` option, you can use desktop applications as well as command-line applications. Desktop applications will be forwarded to your desktop as in remote desktop, VNC or team viewer session. In conjunction with compress `-C` option it will be faster on slow connections (for example, on slow internet), but if you have a local fast network (for example, lan or intranet) it won't work better, because compression/decompression is a CPU cost. So use `-C` option cautiously.

But this method will be better!

[hamachi download page]: https://www.vpn.net/linux
[logmein hamachi user guide]: https://secure.logmein.com/welcome/documentation/EN/pdf/Hamachi/LogMeIn_Hamachi_UserGuide.pdf

Happy coding.

{% include share-post-on-twitter.html %}