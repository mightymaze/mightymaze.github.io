---
layout: post
title: Mastering UFW on Ubuntu
date: '2015-05-16 02:50:22'
tags:
- linux
---

A month before, I have purchased a node in Digital Ocean. I have to admit it, they are a very good cloud solution provider. From a tiny startup to a large corporate, they have any type of solution. I have chosen Ubuntu 14.4 to go with the Digital Ocean droplet. Why I chose Ubuntu! Because of the vast amount of packages available for it via Aptitude.

After having my droplet booted up, there are a lot of work to do. I started with installing the UFW.

![](/content/images/2015/May/Firewall-Installation6.jpg)

###What is UFW?
UFW stands for Uncomplicated Firewall, is a front-end to iptables. It will manage your firewall while providing a very easy interface. It's supported and popular on most of the Linux distos.

###How to install UFW
UFW can be installed using aptitude. The following command will install UFW.

<pre class="brush:shell;">
$ sudo apt-get install ufw
</pre>

###Start the UFW
Current status of UFW can be seen using the command below.
<pre class="brush:shell;">
$ sudo ufw status
</pre>

To start and stop UFW the following commands are used below.
<pre class="brush:shell;">
$ sudo ufw enable
$ sudo ufw disable
</pre>

###Using IPv6
To enable IPv6 support we need to modify the **/etc/default/ufw** file.
<pre class="brush:shell;">
$ sudo vi /etc/default/ufw
</pre>
Then make sure "IPV6" is set to "yes", like:
```
IPV6=yes
```
Now restart the UFW.

###Setup defaults
After installing and starting the UFW, the first task is to setup the default rules. To deny all incoming and outgoing connections I have executed:
<pre class="brush:shell;">
$ sudo ufw default deny incoming
$ sudo ufw default deny outgoing
</pre>

if you do not want to be so much restrictive, then you can allow all outgoing connections.

###Some defaults settings
After done so, I needed to configure UFW to continue working. Like allowing SSH, WWW etc.
<pre class="brush:shell;">
$ sudo ufw allow ssh
$ sudo ufw allow www
</pre>

If you want to allow other ports, follow the command:
<pre class="brush:shell;">
$ sudo ufw allow 8000/tcp
</pre>

Update your port in place of 8000 and udp if needed in place of tcp. If you need to set rules for port range:
<pre class="brush:shell;">
$ sudo ufw allow 8000:9000/tcp
</pre>

Setting rules for a specific IP address is also possible:
<pre class="brush:shell;">
$ sudo ufw allow  from 192.168.255.255
</pre>
Update your IP as needed.

###Deny a connection
Setting rules to deny is almost like the allow command. We need to replace the allow with deny like:
<pre class="brush:shell;">
$ sudo ufw deny 8000/tcp
</pre>

###Deleting an existing rule
Deleting a rule is also possible.
<pre class="brush:shell;">
$ sudo ufw delete allow ssh
$ sudo ufw delete allow 80/tcp
$ sudo ufw delete allow 1000:2000/tcp
</pre>
If this way of deleting seems difficult to you, then you can get the current rules in a numbered list and use that number to delete the rule like:
<pre class="brush:shell;">
$ sudo ufw status numbered
$ sudo ufw delete [number]
</pre>

###Problem with APT-GET
As I have denied all the connection now, apt-get will not work because of UFW. To fix this issue I have added the following rules:

<pre class="brush:shell;">
ufw allow http
ufw allow 53
ufw allow out http
ufw allow out 53
</pre>

###RESET everything
If you need to reset the total configuration then:
<pre class="brush:shell;">
$ sudo ufw reset
</pre>

If you are working on a desktop (not a server), and you have access to a GUI, then you can use GUFW to manage all these settings from a graphical interface. Get it from here [http://gufw.org/](http://gufw.org/)

That's it. That's how to work with UFW on an Ubuntu platform.