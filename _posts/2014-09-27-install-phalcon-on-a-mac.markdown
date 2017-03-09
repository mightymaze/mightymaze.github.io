---
layout: post
title: Install Phalcon on a Mac
date: '2014-09-27 08:59:54'
tags:
- mac
- php
---

If you are thinking of what the Phalcon actually is, Phalcon is a web framework implemented as a C extension offering high performance and lower resource consumption.

![alt]({{ site.url }}/assets/images/2014/Sep/phalcon.jpg)

In a simple word, it's a PHP web framework. It should be installed as a PHP module (.so, .dll). Before installing, the module can be compiled from the source. You may wonder why should we do that for! To run Phalcon we need its platform specific compiled file. If we don't get one, then we have no way but to compile it from the source ourselves.

Okay, now let's go to the point. I have a MacBook Pro and I'm using OSX Mavericks. PHP 5.4, Homebrew and Xcode are installed from before. So to install the Phalcon I've run:

<pre class="brush: shell;">
$ brew search phalcon
$ brew install php54-phalcon
</pre>

The Phalcon for PHP 5.4 finished the installation process. At the end of the installation I got a message like this:

<pre>
To finish installing phalcon for PHP 5.4:
  * /usr/local/etc/php/5.4/conf.d/ext-phalcon.ini was created,
    do not forget to remove it upon extension removal.
  * Validate installation via one of the following methods:
  *
  * Using PHP from a webserver:
  * - Restart your webserver.
  * - Write a PHP page that calls "phpinfo();"
  * - Load it in a browser and look for the info on the phalcon module.
  * - If you see it, you have been successful!
  *
  * Using PHP from the command line:
  * - Run "php -i" (command-line "phpinfo()")
  * - Look for the info on the phalcon module.
  * - If you see it, you have been successful!
</pre>

I've opened the `/usr/local/etc/php/5.4/conf.d/ext-phalcon.ini` file and found:

<pre>
[phalcon]
extension="/usr/local/Cellar/php54-phalcon/1.3.1/phalcon.so"
</pre>

So what I need now is to copy the lines from this file to the end (or somewhere) of my php.ini file. To find where my php.ini is I run:

<pre class="brush: shell;">
php --ini
Configuration File (php.ini) Path: /etc
Loaded Configuration File:         /etc/php.ini
Scan for additional .ini files in: /Library/Server/Web/Config/php
Additional .ini files parsed:      (none)
</pre>

I opened the `/etc/php.ini` file and put the two lines at the end of the file and saved it, Thus the module is installed. Now I need to check it if it's working properly. I've created an index.php containing `phpinfo()` and run an internal php server like this:

<pre class="brush: shell;">
$ nano index.php

GNU nano 2.0.6      File: index.php
&lt;?php
phpinfo();

$ php -S localhost:3000
</pre>

Pointing the browser at `localhost:3000` I've found that the Phalcon module is installed and working perfectly.

<pre>
phalcon

Phalcon Framework	enabled
Phalcon Version	1.3.1

Directive	Local Value	Master Value
phalcon.db.escape_identifiers	On	On
phalcon.orm.column_renaming	On	On
phalcon.orm.enable_literals	On	On
phalcon.orm.events	On	On
phalcon.orm.exception_on_failed_save	Off	Off
phalcon.orm.not_null_validations	On	On
phalcon.orm.virtual_foreign_keys	On	On
phalcon.register_psr3_classes	Off	Off
</pre>

That's it. Now we can run any Phalcon applications from our local server.
