---
layout: post
title: How to use JavaScript Promises
date: '2014-12-02 03:26:54'
---

Javascript Promises are cool new feature of Javascript. A promise is a result of an asynchronous operation. It can be one of the three states.

- Pending: The initial state of a promise. That means it's processing.
- Fulfilled: The operation is successful.
- Rejected: The operation is failed.

Let's consider a code snippet below:

<pre class="brush:js;">
fs.readFile(filename, enc, function (err, res){
  // do our things here
});
</pre>

If we are using generator based flow control this function should be thunkified (thunkify module or similar wrapper) first. But we can implement a Promise and make it generator friendly, which is more convenient in many ways. Let's have a look at the code snippet below:

<pre class="brush:js;">
function readFile(filename, enc) {
  return new Promise(function(fulfill, reject) {
    fs.readFile(filename, enc, function(err, res) {
      if (err) {
        reject(err);
      } else {
        fulfill(res);
      }
    });
  });
}
</pre>

Now this readFile is generator based flow control friendly. Whenever we need to read file we can simply do it in modern way:

<pre class="brush:js;">
var data = yield readFile(filename, enc);
</pre>

That's it, using promises are simple and pretty fun. We will see how to chain events with Javascript Promises in the next article.