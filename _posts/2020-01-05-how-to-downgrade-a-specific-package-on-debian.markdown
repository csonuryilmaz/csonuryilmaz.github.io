---
layout: post
title:  "How to Downgrade a Specific Package on Debian"
date:   2020-01-05 09:00:00 +0300
categories: debian apt-get apt-cache
comments: true
---

I use [jekyll](https://jekyllrb.com/) for static site generation. Its latest version requires minimum `ruby 2.4.0` as of now. But Debian 9.11 (`stretch`) has `ruby 2.3.3` in its repositories, at the time of writing.

```txt
$ ruby -v
ruby 2.3.3p222 (2016-11-21) [x86_64-linux-gnu]
```

Although next release of Debian (`buster`) has `ruby 2.5.1` it has not been backported to `stretch` by now.

So I decided to use [rvm](https://rvm.io/) to get latest versions of `ruby`.
> RVM is a command-line tool which allows you to easily install, manage, and work with multiple ruby environments ..

When you try to install `rvm`, it gives an error about its dependencies and there are unmet dependencies.

```txt
$ sudo apt install curl g++ gcc autoconf automake bison libc6-dev libffi-dev libgdbm-dev libncurses5-dev libsqlite3-dev libtool libyaml-dev make pkg-config sqlite3 zlib1g-dev libgmp-dev libreadline-dev libssl-dev
...
...
The following packages have unmet dependencies:
 libsqlite3-dev : Depends: libsqlite3-0 (= 3.16.2-5+deb9u1) but 3.27.2-3~bpo9+1 is to be installed
 sqlite3 : Depends: libsqlite3-0 (= 3.16.2-5+deb9u1) but 3.27.2-3~bpo9+1 is to be installed
E: Unable to correct problems, you have held broken packages.
```

Above, we try to install the dependencies required for the RVM utility to build Ruby from source. The problem here is that, my Debian installation has both `stretch` and `stretch-backports` defined in its package repositories and latest version of `libsqlite3-0` is active which came from `stretch-backports`.

Normally it's the desired situation when you use backports repository, but in `rvm` case it makes problem. I don't guess `rvm` won't work with `3.27.2-3~bpo9+1`. It's probably because of a static definition about Debian package requirement which ignores `stretch-backports`. For example, when you're on Debian 9.x (`stretch`) `libsqlite3-0` required version should be `3.16.2-5+deb9u1`. There may be a statement like this.

```txt
$ apt-cache policy libsqlite3-0
libsqlite3-0:
  Installed: 3.27.2-3~bpo9+1
  Candidate: 3.27.2-3~bpo9+1
  Version table:
 *** 3.27.2-3~bpo9+1 100
        100 http://ftp.debian.org/debian stretch-backports/main amd64 Packages
        100 /var/lib/dpkg/status
     3.16.2-5+deb9u1 500
        500 http://ftp.debian.org/debian stretch/main amd64 Packages
```

You can continue to install `rvm` without its dependencies ..

```txt
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
...
gpg: Total number processed: 2
gpg:               imported: 2
$ curl -sSL https://get.rvm.io | bash -s stable
...
GPG verified '/home/onur/.rvm/archives/rvm-1.29.9.tgz'
Installing RVM to /home/onur/.rvm/
    Adding rvm PATH line to /home/onur/.profile /home/onur/.mkshrc /home/onur/.bashrc /home/onur/.zshrc.
    Adding rvm loading line to /home/onur/.profile /home/onur/.bash_profile /home/onur/.zlogin.
Installation of RVM in /home/onur/.rvm/ is almost complete:
...
Thanks for installing RVM 🙏
...
$ source ~/.rvm/scripts/rvm
...
```

.. but practically it doesn't work when you try to install a `ruby` version.

```txt
$ rvm install 2.6.3
...
Updating system.....
Installing required packages: gawk, autoconf, automake, bison, libffi-dev, libgdbm-dev, libncurses5-dev, libsqlite3-dev, libtool, libyaml-dev, sqlite3, libgmp-dev, libreadline-dev, libssl-dev.....
Error running 'requirements_debian_libs_install gawk autoconf automake bison libffi-dev libgdbm-dev libncurses5-dev libsqlite3-dev libtool libyaml-dev sqlite3 libgmp-dev libreadline-dev libssl-dev',
please read /home/onur/.rvm/log/1578168638_ruby-2.6.3/package_install_gawk_autoconf_automake_bison_libffi-dev_libgdbm-dev_libncurses5-dev_libsqlite3-dev_libtool_libyaml-dev_sqlite3_libgmp-dev_libreadline-dev_libssl-dev.log
Requirements installation failed with status: 100.
```

The only way to satisfy dependencies seems to downgrade `libsqlite3-0` package to version which came from `stretch` repository.

```txt
$ sudo apt-get install libsqlite3-0=3.16.2-5+deb9u1
...
The following packages will be DOWNGRADED:
  libsqlite3-0
0 upgraded, 0 newly installed, 1 downgraded, 0 to remove and 8 not upgraded.
Need to get 572 kB of archives.
After this operation, 129 kB disk space will be freed.
...
$ apt-cache policy libsqlite3-0
libsqlite3-0:
  Installed: 3.16.2-5+deb9u1
  Candidate: 3.16.2-5+deb9u1
  Version table:
     3.27.2-3~bpo9+1 100
        100 http://ftp.debian.org/debian stretch-backports/main amd64 Packages
 *** 3.16.2-5+deb9u1 500
        500 http://ftp.debian.org/debian stretch/main amd64 Packages
        100 /var/lib/dpkg/status
```

After downgrade, when you check `rvm` requirements again, its dependencies will be installed successfully.

```txt
$ rvm requirements
RVM used your Gemfile for selecting Ruby, it is all fine - Heroku does that too,
you can ignore these warnings with 'rvm rvmrc warning ignore /home/onur/github/csonuryilmaz/csonuryilmaz.github.io/Gemfile'.
To ignore the warning for all files run 'rvm rvmrc warning ignore allGemfiles'.

Unknown ruby interpreter version (do not know how to handle): RUBY_VERSION.
Checking requirements for debian.
Installing requirements for debian.
Updating systemonur password required for 'apt-get --quiet --yes update':
.....
Installing required packages: gawk, autoconf, automake, bison, libffi-dev, libgdbm-dev, libncurses5-dev, libsqlite3-dev, libtool, libyaml-dev, sqlite3, libgmp-dev, libreadline-dev, libssl-dev................
Requirements installation successful.
```

Now, you can install any version of ruby without a problem.

```txt
$ rvm install 2.6.3
Searching for binary rubies, this might take some time.
...
Checking requirements for debian.
Requirements installation successful.
...
Install of ruby-2.6.3 - #complete
Ruby was built without documentation, to build it run: rvm docs generate-ri
$ rvm use 2.6.3 --default
Using /home/onur/.rvm/gems/ruby-2.6.3
$ ruby -v
ruby 2.6.3p62 (2019-04-16 revision 67580) [x86_64-linux]
```

Happy coding.

---

For more and detailed information visit below resources:

- [jekyll requirements and installation](https://jekyllrb.com/docs/installation/)
- [install ruby on debian 9](https://linuxize.com/post/how-to-install-ruby-on-debian-9/)
- [check ruby versions and rvm requirements](https://stackoverflow.com/a/18947112/1750142)
- [search debian packages](https://www.debian.org/distrib/packages#search_packages)
- [useful commands of apt-get and apt-cache for package management](https://www.tecmint.com/useful-basic-commands-of-apt-get-and-apt-cache-for-package-management/)

{% include share-post-on-twitter.html %}
