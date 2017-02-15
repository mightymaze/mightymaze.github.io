---
layout: post
title: Solve Callback Hell in Node.js
date: '2014-07-15 18:49:02'
tags:
- javascript
- node
- js
---

Hello there, how's going! I posted an article one week before. I showed how to deal with [complex queries in MongoDB and Node.js](http://mazharahmed.me/mongodb-complex-query-in-node-js/). At the last of the article we saw that there are so many nested anonymous functions are making our code hardly understandable. We knew that it's called Callback Hell.

Today I'm gonna show you how to solve callback hell issue in Node.js. Any Node.js programmer is familiar with callback hell.

First let's see why callback functions are necessary. Have a look at the code snippet bellow.

<pre class="brush: javascript;">
var result = asyncFunction();
// we will not get the value in result variable
typeof result === 'undefined';  // it's true
</pre>

The execution flow in node will not wait for the async function to be executed fully. To fix this issue we use call back functions like below.

<pre class="brush: javascript;">
asyncFunction(function(result) {
	console.log(result);	// not undefined
});
</pre>

But if our project is a real life project then there will be nested callback functions with a huge depth.

<pre class="brush: javascript;">
someFunction1(function() {
  someFunction2(function() {
    someFunction3(function() {
      someFunction4(function() {
    });
  });
});
</pre>

But writing codes like this is making our code very difficult to understand and maintain. This is callback hell. Now let's figure out how to remove callback hell from our code to make it more understandable.

There are couple of ways to remove callback hell. We can implement new feature of JavaScript, the Generators to get the job done. Or we can use a well known Node.js module to do that. The best solution is to use Generators as it is language feature. But today I'm gonna show you how to do it using the Node.js module <a href="https://www.npmjs.org/package/async" target="_blank">Async</a>.

Async is a Node.js module. We can install it like:

<pre class="brush: shell;">
$ npm install async
</pre>

After successfully installed we can use it like below.

<pre class="brush: javascript;">
var async = require('async');

async.map(['file1','file2','file3'],
fs.stat, function(err, results) {
    // results is now an array of stats
    // for each file
});

async.filter(['file1','file2','file3'],
fs.exists, function(results) {
    // results now equals an array of
    // the existing files
});

async.parallel([
    function() {// code},
    function() {// code}
], callback);

async.series([
    function() {// code},
    function() {// code}
]);
</pre>

So technically our callback hell is solved. The another issue is anonymous functions. We can provide a name to all the anonymous functions to make it more understandable. Providing a name to the anonymous functions is the proof of we care them. And this is a coding standard for Node.js.

<pre class="brush: javascript;">
asyncFunction(function myCallBack(// args) {
	// TODO
});
</pre>

That's it. That's how to solve callback hell from our Node.js codes totally. I will write about Node.js coding standards in my next article. See ya!