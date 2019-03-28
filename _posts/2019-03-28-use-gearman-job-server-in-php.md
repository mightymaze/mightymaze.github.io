---
title: Use Gearman Job Server in PHP
tags:
- php
---

Gearman is a Job Server which has many language interfaces. If you are familiar with Python Celery then you will understand at a glance. Unlike Celery, Gearman is a task runner. The only difference is in Gearman, you have to program your worker too. I love Gearman because not only tasks but also the worker can be written with any suitable language. Also the core of Gearman is built with C/C++, that's why it is very very fast. Say, you are working with PHP and you need a background task runner, go for Gearman. Don't worry, many big companies are using it like  Craig’s List, Tumblr, Yelp, Etsy etc.

![How Does Gearman Work]({{ site.url }}/stack.png)

Now, let's create a Background Task Runner for our PHP Application. To do that we need to install gearman server on our PC / Server first. If we are using Ubuntu then it is very easy like `sudo apt install gearman-job-server` and there are tons of article how to install gearman. Feel free to install it first. Then we have to install the PHP PECL extension for gearman. In Ubuntu we can simply run `sudo pecl install gearman`. Again, there are tons of articles on how to install gearman libraries on your PHP. When you have done with the installations, let's jump directly into coding. Don't forget to run the **gearman job server**.

First lets make the worker. Make a new file named `gearman-worker.php` with:

```php
$worker = new GearmanWorker();
$worker->addServer(); // additional server address can be passed here

$worker->addFunction(
    "ping",
    function (GearmanJob $job) {
        print "received: {$job->handle()}\n";
        
        for ($x = 1; $x < 6; $x++) {
            print "sending data: ping {$x}\n";
            
            $job->sendData("ping {$x}");
            sleep(1);
        }
        
        print "waiting\n";
    }
);

// your other function defination goes here or you may call other files / modules etc

print "waiting\n";

while ($worker->work());
```

That's our simple worker with a function named **ping**. You can add your other functions too. Now let's make a client and call that function we just created. Make a file named `gearman-client.php` with following content:

```php
$client = new GearmanClient();
$client->addServer();

$client->setDataCallback(
    function (GearmanTask $task) {
        print "data: {$task->data()}\n";
    }
);

print "sending\n";

$client->addTask("ping", "hello");
// you can also call tasks async using $client->doBackground()
$client->runTasks();
```

Now we have to keep the worker running. Simply just keep the worker running in our terminal like `php gearman-worker.php` and then run the client from another terminal like `php gearman-client.php`.

If we look at the client terminal its output is like:

```shell
sending
data: ping 1
data: ping 2
data: ping 3
data: ping 4
data: ping 5
```

And the worker terminal will output like:

```shell
waiting
received: H:[network id]:[task id]
sending data: ping 1
sending data: ping 2
sending data: ping 3
sending data: ping 4
sending data: ping 5
waiting
```

> Note: You have to keep the worker running.

This is a simple way of making own worker using Gearman. There are plenty of ways we can use gearman and that's the fun of using it. Hope you guys like it.

Thanks a lot.