---
layout: post
title:  "Extended Cp and Mv Commands"
date:   2018-03-17 00:00:00 +0300
categories: cp mv
comments: true
---

All command line geeks know **cp** and **mv** commands. Especially while copying/moving large files or many files these commands won't verbose anything. So you can't estimate when the job will be finished and watch current progress.

My suggestion is to use **rsync** as a replacement of those commands. By using **bash_aliases** file, we can create short aliases like **cpx** and **mvx** and encapsulate **rsync** details. (**x** is abbreviation of *extended*. I don't want to override original **cp** and **mv**, so I can use orignal ones if necessary.)

First open your favorite text editor and add these lines to your **~/.bash_aliases** file:
{% highlight bash %}
alias cpx='rsync -ah --progress'
alias mvx='rsync -ah --progress --remove-sent-files'
{% endhighlight %}
If your home directory doesn't contain hidden **bash_aliases** file, create a new one.

To enable changes immediately without logout and login to terminal again:
{% highlight bash %}
$ source ~/.bashrc
{% endhighlight %}

* `a` will keep permissions and other file attributes.
* `h` will make output human readable. (size, speed etc.)
* `--progress` will show progress.
* `--remove-sent-files` transforms our **cp** command to **mv** command.

Example, copying a large file:
{% highlight bash %}
$ cpx file.mp4 ~/Downloads/
sending incremental file list
file.mp4
        270.02M 100%   24.60MB/s    0:00:10 (xfr#1, to-chk=0/1)
{% endhighlight %}

Example, moving a large file:
{% highlight bash %}
$ mvx file.mp4 ~/Downloads/
sending incremental file list
file.mp4
        270.02M 100%   41.90MB/s    0:00:06 (xfr#1, to-chk=0/1)
{% endhighlight %}

Example, copying a directory with some files inside:
{% highlight bash %}
$ cpx -r my-pictures ~/Downloads/
sending incremental file list
my-pictures/
my-pictures/Screenshot_2017-09-19_06-07-02.png
        290.36K 100%   81.89MB/s    0:00:00 (xfr#1, to-chk=4/6)
my-pictures/Screenshot_2017-09-19_06-12-24.png
        283.10K 100%   13.50MB/s    0:00:00 (xfr#2, to-chk=3/6)
my-pictures/Screenshot_2017-09-19_06-19-40.png
        291.80K 100%    9.94MB/s    0:00:00 (xfr#3, to-chk=2/6)
my-pictures/Screenshot_2017-09-19_06-34-37.png
        286.78K 100%    6.67MB/s    0:00:00 (xfr#4, to-chk=1/6)
my-pictures/Screenshot_2017-10-10_21-41-43.png
         88.10K 100%    1.91MB/s    0:00:00 (xfr#5, to-chk=0/6)
{% endhighlight %}

Example, moving a directory with some files inside:
{% highlight bash %}
$ mvx -r my-pictures ~/Downloads/
sending incremental file list
my-pictures/
my-pictures/Screenshot_2017-09-19_06-07-02.png
        290.36K 100%  122.83MB/s    0:00:00 (xfr#1, to-chk=4/6)
my-pictures/Screenshot_2017-09-19_06-12-24.png
        283.10K 100%   30.00MB/s    0:00:00 (xfr#2, to-chk=3/6)
my-pictures/Screenshot_2017-09-19_06-19-40.png
        291.80K 100%   21.41MB/s    0:00:00 (xfr#3, to-chk=2/6)
my-pictures/Screenshot_2017-09-19_06-34-37.png
        286.78K 100%   17.09MB/s    0:00:00 (xfr#4, to-chk=1/6)
my-pictures/Screenshot_2017-10-10_21-41-43.png
         88.10K 100%    4.94MB/s    0:00:00 (xfr#5, to-chk=0/6)
{% endhighlight %}

**Note:** `--remove-sent-files` tells rsync to remove from the sending side the files (meaning non-directories) that are a part of the transfer and have been successfully duplicated on the receiving side. So empty directories will be left. Yo should remove them with `rm -Rf` command.

**Columns:** (output of rsync) filename MB-copied MB-speed time-remaining

Happy coding.

{% include share-post-on-twitter.html %}