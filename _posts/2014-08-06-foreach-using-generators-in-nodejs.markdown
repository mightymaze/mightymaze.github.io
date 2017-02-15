---
layout: post
title: forEach using generators in Node.js
date: '2014-08-06 12:27:39'
tags:
- javascript
- node
- js
---

There are many reasons why we wanna run forEach using a generator. Let's imagine of a situation. We are using the Koa framework for our application. We connected to the MongoDB using the Mongoose module.

So anything like this:
<pre class="brush: javascript;">
model.find({...}, function(...) {
	// my code goes here
});
</pre>

is like this for now:
<pre class="brush: javascript;">
var res = yield model.find({...}).exec();
</pre>

So, remember the [complex query executation in mongodb](http://mazharahmed.me/mongodb-complex-query-in-node-js/). So let's see the final outcome from that article. The code looks like:

<pre class="brush: javascript;">
stores.findOne(id, function(fRes) {
  extras.findOne(fRes.id, function(sRes) {
    // So we've all data here
    // merge the results and return
  });
});
</pre>

Let's convert it to use Mongoose and Koa:

<pre class="brush: javascript;">
var store = yield stores.findOne(...).exec();
var extra = yield extras.findOne(...).exec();
</pre>

What if the collections have many to many relations between them. We need to traverse the first result and get the value(s) from the second collection for every entries.

Let's do it using the Mongoose.

<pre class="brush: javascript;">
var store = yield stores.find(...).exec();
store.forEach(function(obj) {
	var extra = yield extras.find(...).exec();
});
</pre>

Unfortunately this code will not work. Because there is a anonymous function in forEach. And we can not use yield from a function. So, how can we solve it! This issue can be fixed in sevaral ways. But the easiest way to fix it is to use a module named '**co**'.

'Co' is a generator based flow-control module for Node.js and the browser. It uses thunks or promises. We can write non-blocking code in a nice-ish way.

How to do that? Let's see.

<pre class="brush: javascript;">
var co = require('co');
var store = yield stores.find(...).exec();
store.forEach(co(function *(obj) {
	var extra = yield extras.find(...).exec();
}));
</pre>

That's it, now the code will run as it should. Isn't that amazing! A small and simple module just saved our day. The modules like 'co', 'thunkify' are very useful. This was just a short example how it can become handy.

There are thousands of modules in npm library. We must get the most possible facilities from them. Before you write a module or library, search if it's already solved by someone else.