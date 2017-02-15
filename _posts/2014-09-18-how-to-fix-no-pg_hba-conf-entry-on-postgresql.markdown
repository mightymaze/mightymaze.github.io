---
layout: post
title: How to fix no pg_hba.conf entry on PostgreSQL
date: '2014-09-18 05:55:22'
tags:
- db
- postgresql
---

Couple of days ago I was deploying a Python app on our VPS and an awkward problem occurred. My app uses PostgreSQL. Whenever I was about to run my app, it crashed right away. I looked at the log and found out that there was an error connecting to my database, so I tried to connect to the database from the shell and found out the error as shown below:

<pre class="brush: shell;">
$ psql --d db-name --U user-name
psql: FATAL:  no pg_hba.conf entry for host "[local]", user "user-name", database "db-name", SSL off
</pre>

The problem was host-based authentication issue. In the config file I missed allowing the user to the database for the host. So the first thing I needed to do is finding the config files. To do that I logged in using the psql shell app and run:

<pre class="brush: shell;">
$ psql --d postgres --u postgres
=#SHOW config_file;
             config_file             
-------------------------------------
 /var/lib/pgsql/data/postgresql.conf
(1 row)

=#SHOW hba_file;
            hba_file             
---------------------------------
 /var/lib/pgsql/data/pg_hba.conf
(1 row)

=# \q
$ _
</pre>

Now that I discovered the locations of config files I checked the postgresql.conf file first if it's loading the pg_hba.conf file properly. I found no problem, then I opened the pg_hba.conf file and found no entry was added. So I added the following lines as below:

<pre>
# TYPE  DATABASE  USER  CIDR-ADDRESS  METHOD
# IPv4 local connections:
host  all  all  127.0.0.1/32  md5
host  all  all  ::1/32  md5
host  confluence  confluence  xxx.xxx.xxx.xxx  md5
# IPv6 local connections:
host  all  all  ::1/128  md5
</pre>

After the modification is done and saved the PostgreSQL is needed to be restarted.

<pre class="brush: shell;">
$ service postgresql restart
</pre>

That's it, after these few steps are done I found no problem running my Python application.