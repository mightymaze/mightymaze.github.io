---
layout: post
title: Reset MySQL root Password in OSX
date: '2014-10-05 06:31:17'
tags:
- mac
- mysql
---

A few days ago I forgot the root password of the MySQL, which is running on my Mac. I wasn't working with MySQL for at least a month. So what I needed was to reset the password so that I can use the service as I needed it. Here's how I did it.

The first step is to stop MySQL service. I stoped it like this:

<pre class="brush: shell;">sudo /usr/local/mysql/support-files/mysql.server stop</pre>

Then I need to start it in safe mode:

<pre class="brush: shell;">sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables</pre>
 
This should be an ongoing command until the process is finished so let's open another shell/terminal window, log in with no password:
 
<pre class="brush: shell;">
mysql -u root
UPDATE mysql.user SET Password=PASSWORD('my-new-password') WHERE User='root';
FLUSH PRIVILEGES;
\q
</pre>

if you are using MySQL 5.7 then you have to do this instead of the commands stated above. Because in MySQL 5.7, the password field in mysql.user table is removed, now the field name is 'authentication_string'.

<pre class="brush: shell;">
mysql -u root
UPDATE mysql.user SET authentication_string=PASSWORD('my-new-password') WHERE User='root';
FLUSH PRIVILEGES;
\q
</pre>

Now again I need to start the MySQL server.

<pre class="brush: shell;">
sudo /usr/local/mysql/support-files/mysql.server start
</pre>

After all of these processes was done, I was able to login as root with the new password I just assigned.
