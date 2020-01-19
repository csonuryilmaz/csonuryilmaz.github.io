---
layout: post
title:  "How to Change Root Password on GNU/Linux"
date:   2020-01-19 09:00:00 +0300
categories: debian gnu linux root passwd
comments: true
---

If you forget your **root** password or want to update for some reason, and you have `sudo` right, you can use **passwd** with `sudo` to change the **root** password.

```txt
$ sudo passwd root
[sudo] password for onuryilmaz:
Changing password for user root.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```

After change, test your **new** root password by typing the following command:

```txt
$ su -
Password:
Last login: Mon Jan 13 08:47:09 +03 2020 from 10.10.99.200 on pts/0
Last failed login: Sun Jan 19 14:51:21 +03 2020 from 10.10.10.16 on ssh:notty
There were 55839 failed login attempts since the last successful login.
```

Happy coding.

---

{% include share-post-on-twitter.html %}
