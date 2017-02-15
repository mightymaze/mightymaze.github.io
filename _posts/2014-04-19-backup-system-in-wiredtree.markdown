---
layout: post
title: Backup system in Wiredtree
date: '2014-04-19 17:08:34'
tags:
- wiredtree
- server
- hosting
- dedicated
---

![wiredtree](/content/images/2014/Apr/tree.gif)

As you already know about Wiredtree and their great support system. Just for those who hasn't yet heard about them "They are awesome hosting provider". Have a look here <a href="http://www.wiredtree.com/" target="_blank">here</a>

We've hosted a very great web application in their Dedicated server. And whenever any issue occured they were very very serious about the support tickets. They always gave support in 1 hour.

Recently I've faced a issue about their backup system. There was a backup system which we can schedule. We did this with their help. But for some reason we just lost our database. We looked in their system and they told us that the backup system actually backs up the system files.

Actually backing up system file meant nothing to us web developers. We have github to preserve our source and we wanted them to backup our database. But there was a mistake. They backup the system files.

Luckily we were able to recover the database from our own backup.

I'm telling this here just to ensure that if anyone of you is using their backup system please make sure from them that they are backing up the right files.

Thanks.