---
layout: post
title: Reset MySQL Root Password on Ubuntu
date: '2016-02-28 12:58:48'
tags:
- mysql
- linux
---

**First please ensure that you really forgot the root password**.
<pre class="brush:shell">mysql -u root -p</pre>

If you really forgot the password then resetting the password is needed. Use ```dpkg-reconfigure``` to reconfigure the root user.

####Check the MySQL version
First  of all check the MySQL server version like this:

<pre class="brush:shell">apt-cache policy mysql-server</pre>

the above command gave me output like:

<pre>Installed: 5.5.37-0ubuntu0.12.04.1</pre>

####Start reconfiguring the MySQL
Start using the following shell command:

<pre class="brush:shell">sudo dpkg-reconfigure mysql-server-*.*</pre>

Please, put your MySQL version name instead of mysql-server-*.* . For me I have used mysql-server-5.5 . It will stop the database and promt for reconfiguration.

![dpkg-reconfigure](/content/images/2016/Feb/GHEyY.png)

You can now enter your new password for the root user and so on. After the reconfiguration is completed the MySQL server will be automatically started.

Now you can login using your new password like this:

<pre class="brush:shell">mysql -u root -p</pre>

That's all for now. Thank you.