---
author: Soumik Ghosh
date: '2011-04-22 05:42:23'
layout: post
comments: true
slug: running-cgi-scripts-using-thttpd
status: publish
title: Running CGI scripts using thttpd
wordpress_id: '58'
categories:
- Server Configuration
tags:
- bash
- cgi
- perl
- python
- thttpd
---

Nginx cannot run CGI scripts natively. You have to either use the [Perl-FastCGI daemon][1] to run your Perl scripts, or for a more "native" CGI
environment, proxy the CGI scripts to a web-server that has the ability to
execute them natively. Apache, surely, will be overkill for this purpose.
Keeping in mind the low-memory requirements of most users who run Nginx, we'll
use thttpd. [thttpd][2] is a tiny, lightweight server (only around 800 kB RSS)
which has the ability to execute CGI scripts natively.

First, let us install thttpd. thttpd hasn't been in active development since
late 2003 (v2.25b), but there isn't any security issue with it. The only issue
is that it doesn't respect (or pass on) the [X-Forwarded-For][3] header. So,
if you are using Debian 5.0.X Lenny, please build from source after applying
[this patch][4] or install from the Squeeze repositories using [apt-pinning][5] (This issue was fixed in [v2.25b-10][6]) .

Assuming you are using Debian Squeeze,
	apt-get install thttpd

Replace the thttpd configuration file with our own :

	mv /etc/thttpd/thttpd.conf /etc/thttpd/thttpd.conf.orig
	vi /etc/thttpd/thttpd.conf

Put this in the file :

    
    host=127.0.0.1
    port=8000
    dir=/var/www  #change this to the $document_root from nginx config file
    user=www-data
    cgipat=**.cgi|**.pl
    logfile=/var/log/thttpd.log
    pidfile=/var/run/thttpd.pid

  
Now let us start thttpd.

	invoke-rc.d thttpd start

We also need to configure nginx to pass on the requests for CGI scripts to
thttpd. Add this to your configuration file :

    
    location ~ \.cgi|pl$ {
         proxy_pass   http://127.0.0.1:8000;
         include proxy.conf;
    }

  
We also need to create a file with some configuration parameters for nginx :

	vi /etc/nginx/proxy.conf

Put this in the file :

    
    proxy_redirect          off;
    proxy_set_header        Host            $host;
    proxy_set_header        X-Real-IP       $remote_addr;
    proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
    client_max_body_size    10m;
    client_body_buffer_size 128k;
    proxy_connect_timeout   90;
    proxy_send_timeout      90;
    proxy_read_timeout      90;
    proxy_buffers           32 4k;

  
Now let us reload the Nginx configuration :

	nginx -s reload

CGI scripts should be now transparently proxied to and executed by thttpd
running on port 8000.

   [1]: http://www.nginxlibrary.com/perl-fastcgi/ (Setting up Perl FastCGI with Nginx)
   [2]: http://www.acme.com/software/thttpd/ (thttpd home page)
   [3]: http://en.wikipedia.org/wiki/X-Forwarded-For
   [4]: http://wiki.nginx.org/index.php?title=ThttpdRealIP&action=raw&anchor=thttpd.patch
   [5]: http://jaqque.sbih.org/kplug/apt-pinning.html (Apt-Pinning)
   [6]: http://packages.debian.org/changelogs/pool/main/t/thttpd/thttpd_2.25b-11/changelog#versionversion2.25b-10

