---
layout: post
title: How to setup a Cron job in Ubuntu
date: '2016-02-18 02:52:20'
tags:
- linux
---

Most of my friends ask me how to setup a cron job in Ubuntu. They often face this situation when they have to setup a cron job for their application. I myself also work with the cron job feature according to my application's demand. Here is how to setup a cron job in Ubuntu in less than a minute.

![Ubuntu]({{ site.url }}/assets/images/2016/Feb/ubuntu-wallpaper-hq-wallpaper.jpg)

You can put a shell script in one of these folders: `/etc/cron.daily`, `/etc/cron.hourly`, `/etc/cron.monthly` or `/etc/cron.weekly`. This should run your script according to the folder name. Such as the scripts in cron.daily folder will be run on daily basis.

If these are not enough for you then you can define your own using the crontab.

```
$ crontab -e
```

This will open personal crontab (cron configuration file) for you. Every lines are tasks that will be run according to the configuration written on that line. The lines are written in this format:

```
minute hour day-of-month month day-of-week command
```

For every number you can define a list by separating the numbers using a comma (,) . You can also use intervals like */20 which will run every 20th. If we put */20 at the minutes field then it will be equivalent to 0,20,40.

Suppose:

```
* 0 * * * path/to/command
*/15 * * * * path/to/command2
```

The first command will be run once a day when the time is 0 that means 12am. The second command will be run on every 15 minutes interval.

That's all guys, that's how to setup a cron job in Ubuntu. Thank you.
