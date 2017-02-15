---
layout: post
title: MongoDB driver on MAMP
tags:
- mongo
- mongodb
- php
- yosemite
---

<pre>
/Applications/MAMP/bin/php/php5.4.4/include/php/Zend/zend.h:51:11: fatal error: 
      'zend_config.h' file not found
# include < zend_config.h >
</pre>

I finally managed to install xdebug. In fact, I had to install Xcode developer tool and then copy the files from /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.10.sdk/usr/include/php into /Applications/MAMP/bin/php/php5.5.18/include and then it worked !


http://lukepeters.me/blog/setting-up-mongodb-with-php-and-mamp