---
layout: post
title: How to run a Basic HTTP server
date: '2014-08-22 08:42:38'
tags:
- javascript
- node
- js
- python
- php
---

There are always situations for which we need to create a Basic HTTP server on our current directory. If we have Apache like web server installed on our system then we can use that by copying our content to a directory in the root of our web directory. We can upload our script to a server and access remotely too.

But all these things seems a bit pain to me. I wanted a simple web server which will run on my current diretory. And ofcourse I want to control the service from the shell. And I wanted a platform independant solution. So I came up with these (from shell):


###Python 2.x

`$ python -m SimpleHTTPServer`


###Python 3.x

`$ python -m http.server`


###PHP

`$ php -S localhost:8000`


###Node Module

This is the best solution I think.

`$ npm install -g instant-server
$ instant <port>`

That's it. Test out the web server at localhost on port 8000. Example: from browser hit the url "[localhost:8000](http://localhost:8000)"