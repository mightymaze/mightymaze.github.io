---
layout: post
title: '"elseif" or "else if", what to use in PHP'
date: '2014-12-21 02:22:37'
tags:
- php
---

Somedays ago one of my teammate came to me asking what's the differences between '**elseif**' and '**else if**' in PHP. They do the same thing according to his view.

So guys, let's see what's the differences.

In PHP, you can write '**else if**' (in two words) and the behavior would be identical to the one of '**elseif**' (in a single word). The syntactic meaning is different (if you're familiar with C, this is the same behavior) but they will provide the same output as the result.

Essentially, they will behave the same, but else if is technically equivalent to a nested structure like below:

<pre class="brush:php;">
if (first_condition)
{

}
else
{
  if (second_condition)
  {

  }
}
</pre>

Note that **elseif** and **else if** will work exactly the same way when using curly brackets as in the above example. When using a colon to define your if/elseif conditions, you can no longer separate else if into two words, or PHP will fail with a parse error.

<pre class="brush:php;">
if (first_condition)
{

}
elseif (second_condition)
{

}
</pre>

Either **elseif** or **else if** can be used. However, if you use the alternate syntax, you must use **elseif**:

<pre class="brush:php;">
if (first_condition):
  // ...
elseif (second_condition):
  // ...
endif;
</pre>

So, for better coding style, we should write as **elseif** and never use **else if** when we are working with a team. It will save time of the others.