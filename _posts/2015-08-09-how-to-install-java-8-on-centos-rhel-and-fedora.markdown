---
layout: post
title: How to Install JAVA 8 on CentOS, RHEL and Fedora
date: '2015-08-09 02:30:21'
tags:
- shell
- linux
---

Recently I was instructed to install Java 8 on our servers. There are plenty of reasons for doing that. Here's how I have done that. Of-course you will need root access to your server. If your are using a shared hosting, then it will be wise to leave this thing to your hosting authority.



###Download the Java 8
Lets keep the java source to our `/opt/` directory.

####For 64 Bit Servers
```
# cd /opt/
# wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u40-b25/jdk-8u40-linux-x64.tar.gz"

# tar xzf jdk-8u40-linux-x64.tar.gz
```

####For 32Bit Servers
```
# cd /opt/
# wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u40-b25/jdk-8u40-linux-i586.tar.gz"

# tar xzf jdk-8u40-linux-i586.tar.gz
```


###Install Java 8
Now install it from the `jdk1.8.0_40` directory.

```
# cd /opt/jdk1.8.0_40/
# alternatives --install /usr/bin/java java /opt/jdk1.8.0_40/bin/java 2
# alternatives --config java


There are 3 programs which provide 'java'.

  Selection    Command
-----------------------------------------------
*  1           /opt/jdk1.8.0/bin/java
 + 2           /opt/jdk1.8.0_25/bin/java
   3           /opt/jdk1.8.0_40/bin/java

Enter to keep the current selection[+], or type selection number: 3
```

Now install rest of the utilities.

```
# alternatives --install /usr/bin/jar jar /opt/jdk1.8.0_40/bin/jar 2
# alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_40/bin/javac 2
# alternatives --set jar /opt/jdk1.8.0_40/bin/jar
# alternatives --set javac /opt/jdk1.8.0_40/bin/javac 
```

At this moment we have installed Java 8. So lets verify it if it's working properly.

```
root@tecadmin ~# java -version

java version "1.8.0_40"
Java(TM) SE Runtime Environment (build 1.8.0_40-b25)
Java HotSpot(TM) 64-Bit Server VM (build 25.40-b25, mixed mode)
```

###Setup system variables
Setup JAVA_HOME Variable
```
# export JAVA_HOME=/opt/jdk1.8.0_40
```
Setup JRE_HOME Variable
```
# export JRE_HOME=/opt/jdk1.8.0_40/jre
```
Setup PATH Variable
```
# export PATH=$PATH:/opt/jdk1.8.0_40/bin:/opt/jdk1.8.0_40/jre/bin
```

That's the end. Now Java 8 is installed on your server.