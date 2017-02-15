---
layout: post
title: Deploy apps in Apache and Node.js together
date: '2014-07-24 11:14:35'
tags:
- node
- apache
- vps
---

There are couple of reasons you might wanna try Apache and Node.js together. Suppose you have an application running in Apache. You have a WordPress blog on that server too. Now you got the opportunity to rewrite the application in Node.js. You will not find anything compared to WordPress in Node.js still now.

So the best possible solution is running Apache and Node.js together. Let's figure out how to do that.

First thing is first, get a VPS which offers two or more IP addresses.

Now that you have a VPS with at least two IP addresses you are ready to go for the configuration. From your WHM cPanel go to **Apache Configuration** from **Service Configuration** then open **Reserved IPs Editor**.

Mark / Tick the IP that you **DON'T WANT** the Apache to listen to. Momorize or write the IP address as you will be needing it very soon. Save the configuration by hitting the save button.

Now you need to use that IP to make Node.js server. Install Node.js on your VPS using the SSH. There are thousands of articles over the Internet. Use that IP and open a HTTP service at port 80. Have a look at the sample code below.

<pre class="brush: javascript;">
var http = require('http');

var server = http.createServer(function(req, res) {
  res.writeHead(200);
  res.end('Node.js works!');
});

server.listen(80, 'your-ip-here');
</pre>

Now you can see that the service is up and running. This blog is also running beside Apache, and we don't need the Apache 	**mode_rewrite** module to proxy the port or whatever.