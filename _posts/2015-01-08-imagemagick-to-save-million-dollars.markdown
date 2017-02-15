---
layout: post
title: ImageMagick to save million dollars
date: '2015-01-08 04:21:21'
tags:
- shell
- php
---

![alt](/content/images/2015/Jan/blog.png)

Once in a project client asked us to re-size and watermark an image after uploading. The system was huge and written in many languages. We decided to write this part in PHP so that we can use GD library.

So, we wrote the section using PHP using the power of GD library. Section was working well, testers found no issue at all. Client was happy, we all were happy.

One day a customer of my client came saying that the module doesn't work for him. We analyzed and found that the user was uploading pictures taken from his digital camera without modification. So the script was going Memory Max Out.

We could increase the memory max limit, but that's not the perfect solution. For this issue users were leaving the client's application. So client was losing money and went crazy. After researching the problem, we decided to push all the image work to a third-party program, then we found an amazing thing in the planet named "ImageMagick".

###What is ImageMagick!
ImageMagick is a software suite to deal with images. It can read and write images in many formats (over 100) including DPX, EXR, GIF, JPEG, JPEG-2000, PDF, PNG, Postscript, SVG, and TIFF.

So when we need any image to be modified or to be worked on we pass it to the ImageMagick via shell access from our script like:

<pre class="brush:php;">
// code here
$output = shell_exec ("convert -resize 350x350 $targetFile $mainFile");
if ($output)
{
	// something returned, most probably error
    // handle it here
}
// code here
</pre>

So, ImageMagick saved our million dollars.