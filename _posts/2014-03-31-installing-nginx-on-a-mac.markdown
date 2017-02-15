---
layout: post
title: Installing nginx on a Mac
date: '2014-03-31 05:45:52'
tags:
- nginx
- mac
- install
- brew
- homebrew
---

Our web applications were using Apache. But for performance optimization we needed to take it in nginx server. So I decided to install nginx on my MacBook Pro before going live to see if our application works well in the new web server.

I've tried to install MNPP which I've installed before and worked perfectly. But for some reason (I don't know why) it was not working this time. So I've decided to install nginx using homebrew. Here is the little process to install nginx on your Mac.

####Installing nginx

nginx (pronounced “engine-x”) is a Web server and a reverse proxy server for HTTP, SMTP, POP3 and IMAP protocols, with a strong focus on high concurrency, performance and low memory usage.

- To ensure Apache doesn't load on startup (Optional, I had Apache from before)
<pre class="brush: shell;">$ sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist</pre>

- If you need any third party extensions then install now. This step is optional.
<pre class="brush: shell;">$ curl -s -L -o /tmp/nginx-upload-progress.tar.gz https://github.com/masterzen/nginx-upload-progress-module/tarball/v0.9.0 && mkdir /tmp/nginx-upload-progress && tar zxpf /tmp/nginx-upload-progress.tar.gz --strip-components 1 -C /tmp/nginx-upload-progress && rm /tmp/nginx-upload-progress.tar.gz

$ curl -s -L -o /tmp/nginx-fair.tar.gz http://github.com/gnosek/nginx-upstream-fair/tarball/master && mkdir /tmp/nginx-fair && tar zxpf /tmp/nginx-fair.tar.gz --strip-components 1 -C /tmp/nginx-fair && rm /tmp/nginx-fair.tar.gz </pre>

- Now one giant line of sed regex that will edit the Homebrew formula for nginx to add the additional compile options that we need. Make sure it all gets entered as one line.

<pre class="brush: shell;">$ sed -i '-default' 's/\([[:space:]]*\['\''--\)\(with-webdav\)\('\'',[[:space:]]*"\)\(Compile with support for WebDAV module\)\("\]\)/\1\2\3\4\5,%\1with-realip\3Compile with support for RealIP module\5,%\1with-gzip_static\3Compile with support for Gzip Static module\5,%\1with-uploadprogress\3Compile with support for Upload Progress module\5,%\1with-fair\3Compile with support for Fair module\5,%\1with-mp4\3Compile with support for MP4 module\5,%\1with-flv\3Compile with support for FLV module\5,%\1with-stub_status\3Compile with support for Stub Status module\5/; s/\([[:space:]]* args << "--\)\(with-http_dav_module\)\(" if ARGV.include? '\''--with-\)\(webdav\)\('\''.*\)/\1\2\3\4\5%\1with-http_realip_module\3realip\5%\1with-http_gzip_static_module\3gzip_static\5%\1add-module=\/tmp\/nginx-upload-progress\3uploadprogress\5%\1add-module=\/tmp\/nginx-fair\3fair\5%\1with-http_mp4_module\3mp4\5%\1with-http_flv_module\3flv\5%\1with-http_stub_status_module\3stub_status\5/; y/%/\n/' $(brew --prefix)/Library/Formula/nginx.rb</pre>

- Now we'll install Nginx with our new build options and extensions and start it.

<pre class="brush: shell;">$ brew install nginx --with-realip --with-gzip_static --with-mp4 --with-flv --with-stub_status --with-uploadprogress --with-fair

$ [ $? -eq 0 ] && rm -rf /tmp/nginx-upload-progress /tmp/nginx-fair

$ mkdir -vp $(brew --prefix nginx)/var/{microcache,log,run}</pre>

- <b>If you don't need above extensions and don't have Apache installed before then just start from here.</b>
<pre class="brush: shell;">$ brew install nginx</pre>
- Nginx is compiled, backup the default nginx config
<pre class="brush: shell;">$ mv /usr/local/etc/nginx/nginx.conf /usr/local/etc/nginx/nginx.conf.bak</pre>
- Download our config as follows
<pre class="brush: shell;">$ curl http://realityloop.com/sites/realityloop.com/files/uploads/nginx.conf_.txt > /usr/local/etc/nginx/nginx.conf</pre>
- Set your username in the new config file
<pre class="brush: shell;">$ sed -i -e 's/\[username\]/'`whoami`'/' /usr/local/etc/nginx/nginx.conf</pre>
- Make nginx log files visible in Console app for accessing in future
<pre class="brush: shell;">$ sudo mkdir /var/log/nginx</pre>
- Create the following directorty to stop “"/var/lib/nginx/speed" failed (2: No such file or directory)” error
<pre class="brush: shell;">$ sudo mkdir /var/lib/nginx</pre>

That's it. nginx is installed.

Thank you.