---
layout: post
title: Get 100% height working in CSS
date: '2014-10-30 11:58:31'
---

Couple of days earlier one of my colleagues came to me saying that he cannot make the 100% working for an element.

His html and css looks like:
<pre>
< !DOCTYPE html>
< html lang="en">
< head>
	< meta charset="UTF-8">
	< title>Sample< /title>

	< style>
		html, body{
			padding: 0;
			margin: 0;
		}
		.heightShower  {
			width:30%;
			height: 100%;
			color: #fff;
			background-color: #e0f;
		}
	< /style>
< /head>
< body>
	< div class="heightShower">
		This is the element
	< /div>
< /body>
< /html>
</pre>

But he's not getting 100% height. The problem was 100% height depends on the element's parent element's height, in this case the element is 100% height, but the body and html doesn't have full height of the viewport as there is nothing else in the body. So the fix is simple, put 100% height for the html and body too. The fix is:

<pre class="brush:css;">
/* 100% height fix */
html, body{
	height: 100%;
}
</pre>

So the code now looks like:

<pre>
< !DOCTYPE html>
< html lang="en">
< head>
	< meta charset="UTF-8">
	< title>Sample< /title>

	< style>
		/* 100% height fix */
		html, body{
			height: 100%;
			padding: 0;
			margin: 0;
		}
		.heightShower  {
			width:30%;
			height: 100%;
			color: #fff;
			background-color: #e0f;
		}
	< /style>
< /head>
< body>
	< div class="heightShower">
		This is the element
	< /div>
< /body>
< /html>
</pre>

Now it works like a charm.