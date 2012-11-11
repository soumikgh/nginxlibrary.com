---
author: Soumik Ghosh
date: '2011-07-07 04:20:54'
layout: post
comments: true
slug: php-fastcgi
status: publish
title: Setting up PHP FastCGI
wordpress_id: '109'
categories:
- Server Configuration
tags:
- php
- php-fastcgi
---

In a previous post, we had written about setting up Perl-FastCGI on Nginx.
This article is about setting up PHP-FastCGI. As always, this article assumes
that you are running on Debian Squeeze (or above) and uses aptitude/apt-get
for fetching and installing packages.

Install these packages :
    
    apt-get install nginx php5-cgi

We'll now replace Nginx's package maintainer's vhosts file with our own.
    
    mv /etc/nginx/sites-available/default /etc/nginx/sites-available/default.old
    vi /etc/nginx/sites-available/default

  
Put this in the file :
  
	server {
	    listen   [::]:80;
	    server_name  example.com; # Replace this with your own
	    root   /var/www/example.com;
	    index  index.html index.htm index.php;
	    access_log  /var/www/logs/example.com.access.log;  

	    location ~ \.php$ {
	        try_files $uri =404;
	        fastcgi_pass   unix:/tmp/php.socket;
	        fastcgi_index  index.php;
	        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
	        include fastcgi_params;
	    }	
	}

Now, there are some tutorials on the internet which wrongly configure Nginx and introduce an arbitrary code execution bug. Note that Nginx passes a request to the php-cgi daemon if the `REQUEST_URI` has a trailing `.php`. It does not check whether the file exists or not. Suppose a request is made for `/images/dog.jpg/test.php`. Nginx will match the trailing `.php` and pass the request to php-cgi. Now, if test.php does not exist and `cgi.fix_pathinfo` is set to 1, php-cgi will  try to execute `/images/dog.jpg` with `/test.php` as pathinfo. This is especially dangerous if you have a public upload directory. In our configuration, this will not happen even if `cgi.fix_pathinfo` is set to 1. The `try_files $uri =404;` line checks for the existence of the .php file and returns a 404 response header if it does not exist. Alternatively, `cgi.fix_pathinfo` can be set to 0, but is is not required as our configuration takes care of it.

Now we'll use a Debian init script to control the php-cgi daemon which will be
running on port 9000.

    vi /etc/init.d/php-fcgi

Add this in the file :

    #!/bin/bash
    ### BEGIN INIT INFO
    # Provides:          php-cgi
    # Required-Start:    networking
    # Required-Stop:     networking
    # Default-Start:     2 3 4 5
    # Default-Stop:      0 1 6
    # Short-Description: Start the PHP FastCGI daemon.
    ### END INIT INFO  
    BIND=/tmp/php.socket
    USER=www-data
    PHP_FCGI_CHILDREN=2
    PHP_FCGI_MAX_REQUESTS=5000  
    PHP_CGI=/usr/bin/php-cgi
    PHP_CGI_NAME=`basename $PHP_CGI`
    PHP_CGI_ARGS="- USER=$USER PATH=/usr/bin PHP_FCGI_CHILDREN=$PHP_FCGI_CHILDREN PHP_FCGI_MAX_REQUESTS=$PHP_FCGI_MAX_REQUESTS $PHP_CGI -b $BIND"
    RETVAL=0  
    start() {
          echo -n "Starting PHP FastCGI: "
          start-stop-daemon --quiet --start --background --chuid "$USER" --exec /usr/bin/env -- $PHP_CGI_ARGS
          RETVAL=$?
          echo "$PHP_CGI_NAME."
    }
    stop() {
          echo -n "Stopping PHP FastCGI: "
          killall -q -w -u $USER $PHP_CGI
          RETVAL=$?
          echo "$PHP_CGI_NAME."
    }  
    case "$1" in
        start)
          start
      ;;
        stop)
          stop
      ;;
        restart)
          stop
          start
      ;;
        *)
          echo "Usage: php-fcgi {start|stop|restart}"
          exit 1
      ;;
    esac
    exit $RETVAL

This can also be done in one line if you have shell access on your server :

    wget http://nginxlibrary.com/downloads/php-fcgi/php-fcgi -O /etc/init.d/php-fcgi

Give the file executable permissions and make it start during booting :

    chmod +x /etc/init.d/php-fcgi && insserv php-fcgi

Finally, start the fastcgi daemon :

    invoke-rc.d php-fcgi start

To check that it is working properly, create this file :

    vi /var/www/example.com/info.php

Put this in it :

	<?php phpinfo(); ?>

Now go to `http://example.com/info.php` and you'll get the PHP information
page showing information about the loaded modules.

Now, there are a couple of options you can change in the PHP-FastCGI init
script, namely, `PHP_FCGI_CHILDREN` and `PHP_FCGI_MAX_REQUESTS`.
`PHP_FCGI_CHILDREN` specifies the number of child processes the php-cgi master
process will spawn. Usually, two, or, even one is sufficient for a medium-
traffic site. `PHP_FCGI_MAX_REQUESTS` specifies the number of requests each
child process will serve before being terminated and respawned.

