---
layout: post
title: How to setup nginx, php and mysql on Ubuntu
tags:
- nginx
- php
- mysql
- linux
---

Recently I was assigned to setup Nginx, PHP and MySQL on a Ubuntu node. There was couple of reasons to choose this environment I think you guys already familier with.

First thing I did is to update my APT and install the Nginx using APT-GET. Then I started the Nginx server and installed PHP-fpm for this server.

```
sudo apt-get update
sudo apt-get install nginx
sudo service nginx start
sudo apt-get install php5-fpm
```

```
# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
#
#location ~ \.php$ {
#       fastcgi_split_path_info ^(.+\.php)(/.+)$;
#        # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
#
#        # With php5-cgi alone:
#       fastcgi_pass 127.0.0.1:9000;
         # With php5-fpm:
#       fastcgi_pass unix:/var/run/php5-fpm.sock;
#       fastcgi_index index.php;
#        include fastcgi_params;
#}
```


```
# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
#
location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
#        # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
#
#        # With php5-cgi alone:
#       fastcgi_pass 127.0.0.1:9000;
         # With php5-fpm:
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
}
```