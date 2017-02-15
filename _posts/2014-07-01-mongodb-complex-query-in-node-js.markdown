---
layout: post
title: MongoDB complex query in Node.JS
date: '2014-07-01 17:37:30'
tags:
- javascript
- node
- mongo
- mongodb
- js
- query
- db
---

As you guys already know that MongoDB isn't suitable for complex queries. There are couple of ways to do it like Map-Reduce. But they will turn the whole application slow. So we can't do that as we use MongoDB and Node.JS for couple of facilities and speed is one of them. So the best solution is to do it programmatically. Let's see how to do it.

Suppose we've two MongoDB collections like:

**Stores**
<pre class="brush: sql;">
{
    "id": "id",
    "name": "store name",
    ...
}
</pre>

**Extras**
<pre class="brush: sql;">
{
    "id": "id",
    "store": "store id here",
    "address": "store address here",
    ...
}
</pre>

So, now we wanna join the two tables to gather the total information about the store. Now let's do our homework, let's try to do this.

###Trial 1

<pre class="brush: javascript;">
// we want our result here
// var store = ...
stores.findOne(id, function gotStore(firstRes) {
	extras.findOne(firstRes.id, function gotExtra(secondRes) {
		// So we've all data here
	});
});
</pre>

Now the problem is how to sync the process so that we can get the data and don't get the NULL. So this system isn't working as we are getting NULL at the end. Let's see what we can do with it.

###Trial 2

As we are using Node.js 0.10 we can't use generators. Let's go deep in the callback hell.

<pre class="brush: javascript;">
function getStore(id, callback) {
    stores.findOne(id, function gotStore(firstRes) {
	    extras.findOne(firstRes.id, function gotExtra(secondRes) {
	    	// So we've all data here
	    	var result = firstRes.concat(secondRes)
	    	callback(result);
	    });
    });
}
</pre>

So now we are getting the values asynchronously we can do our job like this:

<pre class="brush: javascript;">
getStore(100, function printStore(store) {
    console.log (store.name);
});
</pre>

There is almost 4 callback functions. That's Okay for this sample as this sample is short. Real projects are too big and there will be a mess of callback functions. This is called Callback hell.

There are couple of ways to deal with Callback hell. Read my article about [solving Callback Hell in Node.js](http://mazharahmed.me/solve-callback-hell-in-node-js/)

Thanks. Good luck with your own work.