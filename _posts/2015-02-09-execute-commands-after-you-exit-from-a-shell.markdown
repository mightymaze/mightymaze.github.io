---
layout: post
title: Execute commands after you exit from a shell
date: '2015-02-09 10:11:06'
tags:
- shell
- unix
- linux
---

![](/content/images/2015/Feb/Screen-Shot-2015-02-11-at-10-54-07-AM.png)

Most of us used to login to a server using an SSH client. Situations happen like this that a process will take a long time to be finished. System software upgrade is an example of this situation. If we start a command and exit the ssh the process will be stopped immediately. You are probably thinking of running the command on background. But even so, exiting from SSH will terminate the execution. So, is there any solution that the command stays running while we quit the SSH client!

###Meet the `nohup` command

The answer is obvious, use **nohup** command line-utility which allows us to run commands or shell script that can continue running in the background after we log out from a shell.

Use it like:
<pre class="brush:shell;">
$ nohup command-goes-here args &
</pre>

To view active commands:
<pre class="brush:shell;">
$ jobs -l
</pre>

**nohup** doesn't change the scheduling priority of the commands. To change the priority we can use **nice** command with it.

<pre class="brush:shell;">
$ nohup nice -n -5 command-here args &
</pre>

###Queue the command

We can schedule the command for later execution on the server. For example, we can run my-shell-script.sh script to queue (one minute) later execution:
<pre class="brush:shell;">
$ echo "my-shell-script.sh" | at now + 1 minute
</pre>

###Learn to `disown` the shell
We can also use screen command for same purpose. The disown shell internal command can do it for us. Here is how you can try it out:
<pre class="brush:shell;">
$ pullftp.sh &
$ disown -h
$ exit
</pre>

Now we can exit from the SSH. The command will be running on the server.