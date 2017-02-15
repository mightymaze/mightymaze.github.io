---
layout: post
title: Blend modes in CSS
date: '2014-05-19 11:48:38'
tags:
- html5
- css3
- html
- css
- background
- blend
- mode
- photoshop
---

Before I'm gonna say anything I wanna tell you that CSS Background Blend Modes are now available in Chrome Canary and WebKit Nightly. So you need to use one of the supported browsers to see it in action.

First thing first, let's learn what is blend mode and how to use it in CSS.

If you are familiar with Adobe Photoshop from before then you are familiar with the blend mode which we are gonna use. If not then I suggest you to see it from a Photoshop expert.

Let's see how to implement it. Suppose we have  a CSS style like this:

<pre class="brush: css;">
body { 
  background: url('back.png') no-repeat, blue; 
}
</pre>

Now we can use CSS blend mode to this element like this:
<pre class="brush: css;">
body {
  background: url('back.png') no-repeat, blue;
  // Chrome Canary
  background-blend-mode: screen;
  // WebKit Nightly
  -webkit-background-blend-mode: screen;
}
</pre>

The available blend modes are **normal**, **multiply**, **screen**, **overlay**, **darken**, **lighten**, **color-dodge**,  **color-burn**, **hard-light**, **soft-light**, **difference**, **exclusion**, **hue**, **saturation**, **color**, **luminosity**.

This feature isn't enabled in Chrome Canary by default. So <a target="_blank" href="http://html.adobe.com/webplatform/enable/">Learn how to enable flags in Canary to view Background Blend Modes</a>.

If you are using WebKit Nightly then it's enabled by default.

I've created a demo for you <a target="_blank" href="https://dl.dropboxusercontent.com/u/7803124/examples/blend-mode/index.html">here</a>