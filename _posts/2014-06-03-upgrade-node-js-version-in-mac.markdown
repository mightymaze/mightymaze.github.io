---
layout: post
title: Upgrade NodeJS version on Mac
date: '2014-06-03 14:14:59'
tags:
- mac
- install
- mavericks
- lion
- leopard
- javascript
- node
---


![Node.js]({{ site.url }}/assets/images/2014/Jun/nodejs_1280x1024.jpg)

You may wonder how to upgrade the Node.js to the latest version. There's an easy solution, just download and install again. But that's not a smart way to get the things done. We can use NVM to upgrade the version of Node.js. NVM is Node Version Manager. Here's how to use NVM to install Node.js versions. In this example we will install Node.js version 0.11.13.

First install NVM if you didn't.

<pre class="brush: shell;">
$ npm install -g nvm
$ export PATH=./node_modules/.bin:$PATH
</pre>

Now we have installed nvm in our Mac. Let's install 0.11.13 version of Node.js.

<pre class="brush: shell;">
$ nvm install.0.11.13
</pre>

There's another problem with this method. The 0.11.13 version isn't stable. So, you are probably thinking that replacing the stable version might be risky. There's another solution to this problem. We can install multiple versions of Node.js and activate which one is needed. We can do this using <a href=" https://github.com/visionmedia/n" target="_blank">n</a> module. N is another Node.js version manager. It's used to quickly install Node.js versions.

<pre class="brush: shell;">
$ npm install -g n
$ n 0.11.13
</pre>

That's it. Now you can switch between Node.js versions. Just run:

<pre class="brush: shell;">
$ n
</pre>

And select desired Node.js version.

That's it. That's how to upgrade Node.js version on a Mac.
