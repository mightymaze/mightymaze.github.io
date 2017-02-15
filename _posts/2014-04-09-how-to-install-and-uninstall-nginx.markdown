---
layout: post
title: How to Install and Uninstall nginx
date: '2014-04-09 05:43:14'
tags:
- nginx
- mac
- mavericks
---

Install
<pre class="brush: shell;">
cd /usr/local/src
wget http://nginxcp.com/nginxadmin2.3-stable.tar
tar xf nginxadmin2.3-stable.tar
cd publicnginx
./nginxinstaller install
</pre>

Uninstall
<pre class="brush: shell;">
cd /usr/local/src
wget http://nginxcp.com/nginxadmin2.3-stable.tar
tar xf nginxadmin2.3-stable.tar
cd publicnginx
./nginxinstaller uninstall
</pre>