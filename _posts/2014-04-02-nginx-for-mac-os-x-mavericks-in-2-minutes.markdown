---
layout: post
title: Nginx for Mac OS X Mavericks in 2 minutes
date: '2014-04-02 04:03:20'
tags:
- nginx
- mac
- install
- mavericks
- lion
- mountain
- snow
- leopard
- shell
- command
- sh
- kelvin
---

Actually the post is <a href="http://kevinworthington.com/nginx-for-mac-os-x-mavericks-in-2-minutes/" target="_blank">here</a>

KEVIN WORTHINGTON posted the shell commands in a sh file for us to save our time. I'm just putting it in my blog so that I don't forget it in time.

Here are those:

* Snow Leopard <a href="https://dl.dropboxusercontent.com/u/7803124/nginx-osx/build-nginx-snow-leopard.sh" target="_blank">in a nice little script</a>
* Lion <a href="https://dl.dropboxusercontent.com/u/7803124/nginx-osx/build-nginx-osx-lion.sh" target="_blank">in a nice little script</a>
* Mountain Lion <a href="https://dl.dropboxusercontent.com/u/7803124/nginx-osx/build-nginx-mountain-lion.sh" target="_blank">in a nice little script</a>
* Mavericks <a href="https://dl.dropboxusercontent.com/u/7803124/nginx-osx/build-nginx-mac-os-x-mavericks.sh" target="_blank">in a nice little script</a>

Don't forget to correct the permissions of the shell script files before running them. And please run them as root ( sudo ).

like:

<pre class="brush: shell;">$ chmod a+x build-nginx-mac-os-x-mavericks.sh
$ sudo ./build-nginx-mac-os-x-mavericks.sh</pre>

Thanks.