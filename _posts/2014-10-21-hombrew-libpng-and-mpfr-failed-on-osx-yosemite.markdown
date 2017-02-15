---
layout: post
title: Hombrew libpng and mpfr failed on OSX Yosemite
date: '2014-10-21 03:59:42'
tags:
- mac
- homebrew
- yosemite
---

![OSX Yosemit](/content/images/2014/Oct/os_x_yosemite_roundup.jpg)

Two or three days ago I upgraded my OSX to the Yosemite. It's pretty awesome (you may complain, but I will ignore). After upgrading my system, I tried to install Phalcon PHP using my Homebrew and I got a problem with libpng. I got an error like this:

<pre class="brush:shell;">
You must install libpng and mpfr ....
$ brew link libpng
Linking /usr/local/Cellar/libpng/1.6.12... Error: No such file or directory -/usr/local/Cellar/libpng/1.6.10/include/libpng16

$ brew link mpfr
Linking /usr/local/Cellar/mpfr/3.1.2-p8... Error: No such file or directory - /usr/local/Cellar/mpfr/3.1.2/share/doc/mpfr
</pre>

So I'm stuck. The solution I executed is:

<pre class="brush:shell;">
$ brew prune
$ brew remove libpng mpfr
$ sudo echo export PATH='/usr/local/sbin:$PATH' >> ~/.bash_profile
$ brew install freetype
$ brew prune
$ brew install libpng mpfr
</pre>

That's it, after running these I was able to install Phalcon PHP via Homebrew.